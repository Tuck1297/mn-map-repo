<script>
import mapTableOverlay from './mapOverlayTable.vue'
import mapLoadingAnimation from './loadingAnimation.vue'
import mapSidebar from './mapSidebar.vue'

import { toRaw } from 'vue';
import "leaflet.smooth_marker_bouncing";

import { initializeApp } from "firebase/app";
import { getFirestore, doc, getDoc } from "firebase/firestore"
import {initializeAppCheck, ReCaptchaV3Provider} from 'firebase/app-check'


L.Marker.setBouncingOptions({
  bounceHeight: 10, // height of the bouncing
  bounceSpeed: 90, // bouncing speed coefficient
  elastic: false
});

export default {
  props: ['buildmap'],
  data() {
    return {
      // https://{s}.tile.thunderforest.com/transport-dark/{z}/{x}/{y}.png?apikey=13efc496ac0b486ea05691c820824f5f
      // https://{s}.tile.thunderforest.com/outdoors/{z}/{x}/{y}.png?apikey=13efc496ac0b486ea05691c820824f5f
      leaflet: {
        map: null,
        mapBuilt: false,
        click_search_marker: null,
        currCoords: [46.731326037758066, -94.43003354953274],
        osmLayer: L.tileLayer('https://{s}.tile-cyclosm.openstreetmap.fr/cyclosm/{z}/{x}/{y}.png', {
          maxZoom: 18,
          minZoom: 6,
          attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
        }),
        satLayer: L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
          maxZoom: 18,
          minZoom: 6,
          attribution: 'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community'
        }),
        osm2Layer: L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
          maxZoom: 18,
          minZoom: 6,
          attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
        }),
        spicon: null,
        sricon: null,
        swicon: null,
        sficon: null,
        searchIcon: null,
        npsIcon: null,
        campIcon: null,
        markerClusterGroupLand: L.markerClusterGroup({
          showCoverageOnHover: false,
          disableClusteringAtZoom: 9,
          spiderfyOnMaxZoom: false
        }),
        markerClusterGroupCamp: L.markerClusterGroup({
          iconCreateFunction: function (cluster) {
            var markers = cluster.getAllChildMarkers();
            var html = '<div class="circle"><img id="tent_img" src="../../images/tent.png"><span>' + cluster.getChildCount() + '</span></div>';
            return L.divIcon({ html: html, className: 'mycluster', iconSize: L.point(40, 40) });
          },
          showCoverageOnHover: false,
          disableClusteringAtZoom: 9,
          spiderfyOnMaxZoom: false
        }),
        mapClickTimer: 0,
        mapClickDelay: 200,
        mapClickPrevent: false,
      },
      firebase: {
        config: {
          apiKey: "key",
          authDomain: "domain",
          projectId: "id",
          storageBucket: "bucket",
          messagingSenderId: "senderid",
          appId: "appid"
        },
        app: null,
        db: null,
        docRef: null,
        docSnap: null, 
        appCheck: null
      },
      landCampVisibleArr: [true, true, true, true, true, false],
      campTypeSpeVis: [true, true, true, true, true, true, true, true],
      stateParkMarkers: [],
      stateRecMarkers: [],
      stateWayMarkers: [],
      stateForestMarkers: [],
      nationalParkMarkers: [],
      campMarkers: [],
      stateParkCampMarkers: [],
      stateForestCampMarkers: [],
      nationalForestCampMarkers: [],
      engineersCampMarkers: [],
      privateCampMarkers: [],
      nationalGuardCampMarkers: [],
      countyCampMarkers: [],
      koaCampMarkers: [],
      searchMarkerTextboxResults: [],
      searchTextboxResults: [],
      stateLandData: [],
      forecastData: [],
      // State Bitmap Bits
      proToursBit: 0,
      thingsToSeeBit: 0,
      campSeasonBit: 0,
      campsiteTypesBit: 0,
      bathFacilitiesBit: 0,
      lodgingBit: 0,
      trailsBit: 0,
      recFacilitiesBit: 0,
      rentalsBit: 0,
      winterRecBit: 0,
      // Camp Bitmap Bits
      campSpecsBit: 0,
      hookupBit: 0,
      sitesHaveBit: 0,
      // campgndLandIsBit: 0,
      displayTable: false // this is for displaying the summarized table data
    }
  },
  computed: {
    computeBuildMap() {
      return this.buildmap;
    }
  },
  watch: {
    computeBuildMap(value) {
      if (value == true && this.leaflet.mapBuilt == false) {
        this.leaflet.spicon = L.icon({
          iconUrl: '../../images/sp3_marker.png',
          iconSize: [25, 35],  // size of the icon
          iconAnchor: [12.5, 35] // point of the icon which will correspond to marker's location
        });
        this.leaflet.sricon = L.icon({
          iconUrl: '../../images/sr3_marker.png',
          iconSize: [25, 35],  // size of the icon
          iconAnchor: [12.5, 35]  // point of the icon which will correspond to marker's location
        });
        this.leaflet.swicon = L.icon({
          iconUrl: '../../images/sw_marker.png',
          iconSize: [25, 35],  // size of the icon
          iconAnchor: [12.5, 35]  // point of the icon which will correspond to marker's location
        });
        this.leaflet.sficon = L.icon({
          iconUrl: '../../images/sf_marker.png',
          iconSize: [25, 35],  // size of the icon
          iconAnchor: [12.5, 35] // point of the icon which will correspond to marker's location
        });
        this.leaflet.searchIcon = L.icon(
          {
            iconUrl: '../../images/curr_marker.png',
            iconSize: [25, 35], // size of the icon
            iconAnchor: [12.5, 35] // point of the icon which will correspond to marker's location
          }
        );
        this.leaflet.npsIcon = L.icon(
          {
            iconUrl: '../../images/nps_marker.png',
            iconSize: [25, 35], // size of the icon
            iconAnchor: [12.5, 35] // point of the icon which will correspond to marker's location
          }
        );
        this.leaflet.campIcon = L.icon({
          iconUrl: '../../images/camp_marker.png',
          iconSize: [25, 35], // size of the icon
          iconAnchor: [12.5, 35] // point of the icon which will coorespond to marker's location
        });

        $(".map_img").each(function (i, obj) {
          $(obj).attr("src", $(obj).attr("data"));
        });

        this.initiateMap();
        this.leaflet.mapBuilt = true;
      } else {
        toRaw(this.leaflet.map).invalidateSize(true);
      }

    }
  },
  mounted() {
    this.firebase.app = initializeApp(this.firebase.config);
    this.firebase.db = getFirestore(this.firebase.app);
    this.firebase.appCheck = initializeAppCheck(this.firebase.app, {
      provider: new ReCaptchaV3Provider('6LcZxLIkAAAAAG-wRt6_CseYUmrWkGXJztv5QNeL'),
      isTokenAutoRefreshEnabled: true
    })
  },
  components: {
    mapTableOverlay,
    mapLoadingAnimation,
    mapSidebar
  },
  methods: {
    // Setting Up Map and Data Handling Methods
    initiateMap() {
      this.leaflet.map = L.map("leafletmap", {
        center: [46.731326037758066, -94.43003354953274],
        zoom: 6,
        layers: [this.leaflet.osmLayer]
      });
      // toRaw(this.leaflet.map).setMaxBounds([[39.499356, -102.239209], [52.384358, -85.483375]])
      toRaw(this.leaflet.map).zoomControl.setPosition('bottomright');
      this.importBorder((data) => {
        let borderStyle = {
          "color": "#4352a5",
          "weight": 2,
          "fillOpacity": 0
        }

        L.geoJSON(data, {
          style: borderStyle
        }).addTo(toRaw(this.leaflet.map));
      });

      var sidebar = L.control.sidebar('sidebar', {
        position: 'left'
      });

      sidebar.addTo(toRaw(this.leaflet.map));

      var basemaps = {
        "Satellite": this.leaflet.satLayer,
        "Street View": this.leaflet.osm2Layer
      }

      var layerControl = L.control.layers(null, basemaps, { position: 'bottomright' });
      layerControl.addTo(toRaw(this.leaflet.map));

      // this.plotCampsiteMarkers('../../data/mn_campgrounds_data.json');
      this.addEventListeners();
      this.createSearchClickMarker([46.731326037758066, -94.43003354953274]);
      // restructure this later so it will disappear either after all data is 
      // loaded and plotted or after 10 seconds -- whichever comes first\/
      setTimeout(() => {
        toRaw(this.leaflet.map).invalidateSize(true);
      }, 200);
      this.plotStateLands('../../data/state_land_data_webserve.json', (loadingState) => {
        if (loadingState) {
          setTimeout(() => {
            $('.loading_slate').css("display", 'none');
          }, 1000);
        }
      });

    },
    importBorder(done) {
      let url = '../../data/mn_border_coords.json';
      this.fetchData(url, (data) => {
        if (data == false) {
          return;
        }
        done(data);
      });
    },
    fetchData(url, done) {
      fetch(url)
        .then(response => response.json())
        .then(data => {
          done(data)
        })
        .catch((error) => {
          console.log(error);
          done(false);
        });
    },
    getWeatherData(url) {
      this.fetchData(url, (data) => {
        if (data == false || data.status == 500) {
          this.forecastData.push({ name: "Unable to retrieve the Weather at this time. Click the marker again to reload the sidebar...", temp: '', windData: '', shortForecast: '', longForecast: '', icon: '../../images/unhappy.png' });
          return;
        }
        this.fetchData(data.properties.forecast, (data) => {

          if (data == false || data.status == 500) {
            this.forecastData.push({ name: "Unable to retrieve the Weather at this time. Click the marker again to reload the sidebar...", temp: '', windData: '', shortForecast: '', longForecast: '', icon: '../../images/unhappy.png' });
            return;
          }
          this.forecastData = [];
          for (let element in data.properties.periods) {
            let currElement = data.properties.periods[element];
            let name = currElement.name;
            let temp = `${currElement.temperature}${currElement.temperatureUnit}`
            let windData = `Wind: ${currElement.windSpeed}, ${currElement.windDirection}`
            let shortForecast = currElement.shortForecast;
            let longForecast = currElement.detailedForecast;
            let icon = currElement.icon;
            this.forecastData.push({ name, temp, windData, shortForecast, longForecast, icon });
          }
        });
      });
    },
    plotStateLands(url, done) {
      this.fetchData(url, (data) => {
        if (data == false) {
          return;
        }
        for (let element in data) {
          let currLand = data[element]
          // Create markers and content that displays when marker is clicked
          let currLandType = currLand.LAND_TYPE;
          let coords = [currLand.LAT, currLand.LONG];
          let landNameMsg = "";
          if (currLand.URL[0] == 'h') {
            landNameMsg = `<a href="${currLand.URL}" target="_blank">
            ${currLand.LAND_NAME} ${currLandType}</a>`;
          } else {
            landNameMsg = `<a href="https://www.dnr.state.mn.us${currLand.URL}" target="_blank">
            ${currLand.LAND_NAME} ${currLandType}</a>`;
          }
          let directionsLink = `<a href="https://www.google.com/maps/dir/?api=1&destination=${currLand.LAT}, ${currLand.LONG}"
          target="_blank">Get Directions</a>`;
          let parkTypeMsg = `Park Type: ${currLand.LAND_TYPE}`;
          let lat_dms = this.calculateDMS(currLand.LAT, 'lat');
          let lon_dms = this.calculateDMS(currLand.LONG, 'lon');

          let address = `${currLand.CITY}, ${currLand.STATE}`;
          let marker;
          let msgArr = [landNameMsg, parkTypeMsg, address, coords, lat_dms, lon_dms, directionsLink, currLand.LAND_NAME, currLand.URL];

          if (currLandType === "State Forest") {
            marker = this.createMarker(coords, currLand.ID, this.leaflet.sficon, this.unpackageBitmap(currLand.CATEGORIES), msgArr, 'park');
            this.stateForestMarkers.push(marker);
          }
          if (currLandType === "State Park") {
            this.unpackageBitmap(currLand.CATEGORIES);
            marker = this.createMarker(coords, currLand.ID, this.leaflet.spicon, this.unpackageBitmap(currLand.CATEGORIES), msgArr, 'park');
            this.stateParkMarkers.push(marker);
          }
          if (currLandType === "State Recreation Area") {
            this.unpackageBitmap(currLand.CATEGORIES);
            marker = this.createMarker(coords, currLand.ID, this.leaflet.sricon, this.unpackageBitmap(currLand.CATEGORIES), msgArr, 'park');
            this.stateRecMarkers.push(marker);
          }
          if (currLandType === "State Wayside") {
            this.unpackageBitmap(currLand.CATEGORIES);
            marker = this.createMarker(coords, currLand.ID, this.leaflet.swicon, this.unpackageBitmap(currLand.CATEGORIES), msgArr, 'park');
            this.stateWayMarkers.push(marker);
          }

          if (currLandType != 'State Wayside' & currLandType != "State Recreation Area" &
            currLandType != 'State Forest' & currLandType != 'State Park') {
            marker = this.createMarker(coords, currLand.ID, this.leaflet.npsIcon, this.unpackageBitmap(currLand.CATEGORIES), msgArr, 'park');
            this.nationalParkMarkers.push(marker);
          }
        }
        toRaw(this.leaflet.map).addLayer(toRaw(this.leaflet.markerClusterGroupLand));
        done(true);
      });
    },
    plotCampsiteMarkers(url, done) {
      this.fetchData(url, (data) => {
        if (data == false) {
          return;
        }
        for (let element in data) {
          let currCamp = data[element]
          // Create markers and content that displays when marker is clicked
          let coords = [currCamp.LAT, currCamp.LONG];
          let landNameMsg = "";
          let landNamePart = "";
          let camp_type = currCamp.CAMP_TYPE;
          if (camp_type == "State Park" || camp_type == "State Forest") {
            landNamePart = camp_type;
          }
          if (currCamp.CAMP_URL[0] == "h") {
            landNameMsg = `<a href="${currCamp.CAMP_URL}" target="_blank">
            ${currCamp.NAME} ${landNamePart}</a>`;
          }
          if (currCamp.CAMP_URL == "") {
            landNameMsg = `${currCamp.CAMP_NAME} ${landNamePart}`;
          }

          let address = `${currCamp.CITY}, ${currCamp.STATE}`;
          let directionsLink = `<a href="https://www.google.com/maps/dir/?api=1&destination=${currCamp.LAT}, ${currCamp.LONG}"
          target="_blank">Get Directions</a>`;
          let parkTypeMsg = `Campground Type: ${currCamp.CAMP_TYPE}`;
          let lat_dms = this.calculateDMS(currCamp.LAT, 'lat');
          let lon_dms = this.calculateDMS(currCamp.LONG, 'lon');
          let marker;
          let msgArr = [landNameMsg, parkTypeMsg, address, coords, lat_dms, lon_dms, directionsLink, currCamp.NAME, currCamp.CAMP_URL];

          marker = this.createMarker(coords, currCamp.ID, this.leaflet.campIcon, this.unpackageCampBitmap(currCamp.BITMAP), msgArr, 'camp');
          toRaw(marker).options.tableData.display = false;

          switch (currCamp.CAMP_TYPE) {
            case "State Park":
              this.stateParkCampMarkers.push(marker);
              break;
            case "State Forest":
              this.stateForestCampMarkers.push(marker);
              break;
            case "National Forest":
              this.nationalForestCampMarkers.push(marker);
              break;
            case "Private":
              this.privateCampMarkers.push(marker);
              break;
            case "Corps of Engineers":
              this.engineersCampMarkers.push(marker);
              break;
            case "KOA":
              this.koaCampMarkers.push(marker);
              break;
            case "County Park":
              this.countyCampMarkers.push(marker);
              break;
            default:
              // default is National Guard Campground
              this.nationalGuardCampMarkers.push(marker);
              break;
          }
        }
        done();
      });
    },
    createMarker(coords, id, icon, bitmapVal, msgArr, type) {
      let marker = new L.marker(coords, { center: false, icon: icon, bitmapVal, tableData: { address: msgArr[2], landType: msgArr[1], landName: msgArr[7], url: msgArr[8], display: true } })
      marker._id = id;
      let dbConnectType = ""

      if (type == 'park') {
        marker.addTo(toRaw(this.leaflet.markerClusterGroupLand));
        dbConnectType = "State_Lands_Data"
      } else {
        marker.addTo(toRaw(this.leaflet.markerClusterGroupCamp));
        dbConnectType = "State_Camp_Data"
      }
      marker.on('click', () => {
        this.flyTo(coords)
        this.leaflet.map.once("moveend", () => {
          marker.bounce(5);
          this.getFirebaseLandData(id, dbConnectType, (dbObject) => {
            // Sidebar Data Injection
            // default values
            $("#header_link").html(msgArr[0]);
            $("#type_id").html(msgArr[1]);
            $("#lat_id").html(msgArr[3][0]);
            $("#lon_id").html(msgArr[3][1]);
            $("#lat_dms").html(msgArr[4]);
            $("#lon_dms").html(msgArr[5]);
            let lat = `${msgArr[3][0]}`;
            let lon = `${msgArr[3][1]}`;

            this.getWeatherData(`https://api.weather.gov/points/${lat.slice(0, 7)},${lon.slice(0, 8)}`);
            // collection specific
            let highlightsArr, highlightsMsg, bookingMsg;
            if (type == 'park') {
              // State_Lands_Data
              $("#phone_id").html(dbObject.PHONE_NUM);
              $("#estab_id").html(`Since: ${dbObject.YEAR_ESTAB}`);
              let address = "";
              if (dbObject.ADDRESS !== "") {
                address = `${dbObject.ADDRESS},`
              }
              $("#address_id").html(`${address} ${msgArr[2]}, ${dbObject.ZIP} <br> Region: ${dbObject.DNR_REGION} <br> County: ${dbObject.COUNTY_NAME} <br> ${msgArr[6]}`);
              $("#desc_id").html(dbObject.DESC);
              highlightsArr = dbObject.HIGHLIGHTS.split("%#%");
              highlightsMsg = "";
              for (let element in highlightsArr) {
                let currElement = highlightsArr[element];
                highlightsMsg += `<li>${currElement}</li>`;
              }
              $("#highlights_id").html(highlightsMsg);
              $("#dnr_id").html(dbObject.PROP_ID);
            } else {
              // State_Camp_Data
              $("#phone_id").html(dbObject.PHONE);
              $("#estab_id").html(`Elevation: ${dbObject.ELEV} <br> Opened: ${dbObject.OPENED}`);
              let address = ""
              if (dbObject.ADDRESS !== "") {
                address = dbObject.ADDRESS + ",";
              }
              bookingMsg = "";
              if (dbObject.BOOKING !== "") {
                bookingMsg = `<a href="${dbObject.BOOKING}" target="_blank">~~Book Your Stay~~</a>`
              }
              $("#address_id").html(`${address} ${msgArr[2]}, ${dbObject.ZIP} <br> ${msgArr[6]}`); // <-- rest from database
              $("#desc_id").html(`${dbObject.DESC} <br> ${bookingMsg}`);
              highlightsArr = dbObject.HIGHLIGHTS.split(",");
              highlightsMsg = "";
              for (let element in highlightsArr) {
                let currElement = highlightsArr[element];
                highlightsMsg += `<li>${currElement}</li>`;
              }
              $("#highlights_id").html(highlightsMsg);
              $("#dnr_id").html(id);
            }
            // Window Operations
            this.closeClickableWindow();
              this.infoSlideActive("marker_content_div");
              $("#backBtn").css("display", "none");
          });
        })
      })
      return marker;
    },
    unpackageBitmap(bitmap) {
      // bitmap --> the values in which define the state land
      let bitmapArr = bitmap.split("%#%");
      return {
        bitmap: {
          natureTours: parseInt(bitmapArr[0], 2),
          locationHighlights: parseInt(bitmapArr[1], 2),
          campSeasons: parseInt(bitmapArr[2], 2),
          campTypes: parseInt(bitmapArr[3], 2),
          campAmenities: parseInt(bitmapArr[4], 2),
          otherStayTypes: parseInt(bitmapArr[5], 2),
          pathTypes: parseInt(bitmapArr[6], 2),
          parkAmenities: parseInt(bitmapArr[7], 2),
          recRent: parseInt(bitmapArr[8], 2),
          winterRec: parseInt(bitmapArr[9], 2)
        }
      }
    },
    unpackageCampBitmap(bitmap) {
      return {
        bitmap: {
          specs: parseInt(bitmap.slice(0, 15), 2),
          hook: parseInt(bitmap.slice(15, 19), 2),
          sitesHave: parseInt(bitmap.slice(19, 27), 2),
          // campgndLandIs: parseInt(bitmap.slice(27, 31), 2),
        }
      }
    },
    calculateDMS(coordinate, type) {
      let degreeArr = coordinate.toString().split(".");
      let degree = degreeArr[0];

      // check the type of coordinate and set the direction
      let direction = (type === 'lat') ? (coordinate > 0 ? "N" : "S") : (coordinate > 0 ? "E" : "W");

      // get absolute value of the coordinate
      let absCoordinate = Math.abs(coordinate);

      // get the degrees
      let degrees = Math.floor(absCoordinate);

      // get the minutes and seconds
      let minutes = Math.floor((absCoordinate - degrees) * 60);
      let seconds = (((absCoordinate - degrees) * 60) - minutes) * 60;

      // return the result as a string
      return `${degree}° ${minutes}' ${seconds.toFixed(3)}'' ${direction}`;
    },
    createSearchClickMarker(coords) {
      let marker = new L.marker(coords, { center: false, icon: this.leaflet.searchIcon });
      marker._id = "Location Marker";
      marker.addTo(toRaw(this.leaflet.map))
        .on('click', () => {
          marker.bounce(3);
        });
      marker.bounce(3);
      this.leaflet.click_search_marker = marker;
    },
    updateSearchClickMarker(coords) {
      toRaw(this.leaflet.click_search_marker).setLatLng(coords);
      toRaw(this.leaflet.click_search_marker).bounce(5);
    },
    fetchOpenStreetData(val, callback) {
      fetch(`https://nominatim.openstreetmap.org/search?q='${val}, Minnesota'&format=json&limit=10&accept-language=en&countrycodes=us`)
        .then(response => response.json())
        .then(data => {
          if (data.length == 0) {
            // alert('Your requested query cannot be fulfilled at this time.');
          }
          callback(data)
        })
        .catch(error => {
          console.log(error);
          // alert('Your requested query cannot be fulfilled at this time.');
        });
    },
    fetchOpenStreetDataFromLatLon(coords, callback) {
      // coords is an array [lon, lat]
      let lat = coords.lat;
      let lng = coords.lng;
      fetch(`https://nominatim.openstreetmap.org/reverse?lat=${lat}&lon=${lng}&format=json`)
        .then(response => response.json())
        .then(data => { callback(data) })
        .catch(error => {
          console.log(error);
          // alert('Your requested query cannot be fulfilled at this time.');
        })
    },
    async getFirebaseLandData(id, collection_access, done) {
      this.firebase.docRef = doc(this.firebase.db, collection_access, `${id}`)
      this.firebase.docSnap = await getDoc(this.firebase.docRef);
      if (this.firebase.docSnap.exists()) {
        // console.log("Document Data: ", this.firebase.docSnap.data());
        done(this.firebase.docSnap.data())
      } else {
        //doc.data() will be undefined in this case
        console.log("Document not found...")
        done(null)
      }
    },
    processAddressData(data) {
      let contentString = '<p class="info_header"></p>';
      if (data != undefined) {
        contentString += `<p class="info_element"><a href="https://www.google.com/maps/dir/?api=1&origin=${data.lat}, ${data.lon}" target="_blank">Get Directions</a></p>`
        let address = data.address;
        for (let element in address) {
          let identifier = element.slice(0, 1).toUpperCase() + element.slice(1, element.length);
          contentString += `<p class="info_element">${identifier}: ${address[element]}</p>`;
        }
        let roundedLat = data.lat.slice(0, 12);
        let roundedLon = data.lon.slice(0, 12);
        contentString += `<p class="info_element">Latitude: <code>${roundedLat}</code></p><p class="info_element">Longitude:  <code>${roundedLon}</code></p>`;
      }
      return contentString;
    },
    // User Interface Methods
    addEventListeners() {
      // Event Listener for map click
      toRaw(this.leaflet.map).on('click', (evt) => {
        this.leaflet.mapClickTimer = setTimeout(() => {
          if (!this.leaflet.mapClickPrevent) {
            this.doSingleClickAction(evt);
          }
          this.leaflet.mapClickPrevent = false;
        }, this.leaflet.mapClickDelay)

      }).on('dblclick', (evt) => {
        clearTimeout(this.leaflet.mapClickTimer);
        this.leaflet.mapClickPrevent = true;
        this.doDoubleClickAction(evt);
      });
      $('#flyBtn').click(() => {
        this.flyTo(this.leaflet.currCoords);
      });
    },
    doSingleClickAction(evt) {
      let coords = evt.latlng;

      if (this.leaflet.click_search_marker === null) {
        this.createSearchClickMarker(coords);
      } else {
        this.updateSearchClickMarker(coords);
      }

      this.fetchOpenStreetDataFromLatLon(coords, (data) => {
        let message = this.processAddressData(data);
        $("#marker_content_click").html(message);
        this.infoSlideActive("marker_content_click_div");
        toRaw(this.leaflet.map).panTo(coords, 14);
        this.leaflet.currCoords = [coords.lat, coords.lng];
      });
    },
    doDoubleClickAction(evt) {
      // console.log("double click")
    },
    turnOn(arr) {
      for (let element in arr) {
        let marker = arr[element];
        marker.options.tableData.display = true;
        toRaw(this.leaflet.markerClusterGroupLand).addLayer(marker);
        toRaw(marker).options.tableData.display = true;
      }
      // this.updateVisibleMarkers();
    },
    turnOnCamp(arr) {
      for (let element in arr) {
        let marker = arr[element];
        marker.options.tableData.display = true;
        toRaw(this.leaflet.markerClusterGroupCamp).addLayer(marker);
        toRaw(marker).options.tableData.display = true;
      }
    },
    turnOff(arr) {
      for (let element in arr) {
        let marker = arr[element];
        marker.options.tableData.display = false;
        toRaw(this.leaflet.markerClusterGroupLand).removeLayer(marker);
        toRaw(marker).options.tableData.display = false;
      }
      // this.updateVisibleMarkers();
    },
    turnOffCamp(arr) {
      for (let element in arr) {
        let marker = arr[element];
        marker.options.tableData.display = false;
        toRaw(this.leaflet.markerClusterGroupCamp).removeLayer(marker);
        toRaw(marker).options.tableData.display = false;
      }
    },
    updateVisibleMarkers() {
      // loop through all the markers where you do an AND with all the bit
      // parts stored in all the markers
      if (this.landCampVisibleArr[0] == true) {
        this.startBitmapCheck(this.stateParkMarkers);
      }
      if (this.landCampVisibleArr[2] == true) {
        this.startBitmapCheck(this.stateRecMarkers);
      }
      if (this.landCampVisibleArr[3] == true) {
        this.startBitmapCheck(this.stateWayMarkers);
      }
      if (this.landCampVisibleArr[4] == true) {
        this.startBitmapCheck(this.nationalParkMarkers);
      }
      if (this.landCampVisibleArr[1] == true) {
        this.startBitmapCheck(this.stateForestMarkers);
      }
      if (this.landCampVisibleArr[5] == true) {
        // this.startCampBitmapCheck(this.campMarkers);
        if (this.campTypeSpeVis[0] == true) {
          this.startCampBitmapCheck(this.stateParkCampMarkers);
        }
        if (this.campTypeSpeVis[1] == true) {
          this.startCampBitmapCheck(this.stateForestCampMarkers);
        }
        if (this.campTypeSpeVis[2] == true) {
          this.startCampBitmapCheck(this.nationalForestCampMarkers);
        }
        if (this.campTypeSpeVis[3] == true) {
          this.startCampBitmapCheck(this.countyCampMarkers);
        }
        if (this.campTypeSpeVis[4] == true) {
          this.startCampBitmapCheck(this.privateCampMarkers);
        }
        if (this.campTypeSpeVis[5] == true) {
          this.startCampBitmapCheck(this.koaCampMarkers);
        }
        if (this.campTypeSpeVis[6] == true) {
          this.startCampBitmapCheck(this.engineersCampMarkers);
        }
        if (this.campTypeSpeVis[7] == true) {
          this.startCampBitmapCheck(this.nationalGuardCampMarkers);
        }
      }

    },
    startBitmapCheck(arr) {
      for (let i = 0; i < arr.length; i++) {
        let currMarker = arr[i];
        let currentBitmaps = currMarker.options.bitmapVal.bitmap;

        if (this.compareBitmaps(currentBitmaps.natureTours, this.proToursBit) != this.proToursBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.locationHighlights, this.thingsToSeeBit) != this.thingsToSeeBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.campSeasons, this.campSeasonBit) != this.campSeasonBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.campTypes, this.campsiteTypesBit) != this.campsiteTypesBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.campAmenities, this.bathFacilitiesBit) != this.bathFacilitiesBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.otherStayTypes, this.lodgingBit) != this.lodgingBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.pathTypes, this.trailsBit) != this.trailsBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.parkAmenities, this.recFacilitiesBit) != this.recFacilitiesBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.recRent, this.rentalsBit) != this.rentalsBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.winterRec, this.winterRecBit) != this.winterRecBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupLand).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }
        // For map
        toRaw(this.leaflet.markerClusterGroupLand).addLayer(toRaw(currMarker));
        // For Table
        toRaw(currMarker).options.tableData.display = true;
      }
    },
    startCampBitmapCheck(arr) {
      // console.log(this.campgndLandIsBit, this.sitesHaveBit, this.hookupBit, this.campSpecsBit)

      for (let i = 0; i < arr.length; i++) {
        let currMarker = arr[i];
        let currentBitmaps = currMarker.options.bitmapVal.bitmap;

        if (this.compareBitmaps(currentBitmaps.specs, this.campSpecsBit) != this.campSpecsBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupCamp).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.hook, this.hookupBit) != this.hookupBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupCamp).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        if (this.compareBitmaps(currentBitmaps.sitesHave, this.sitesHaveBit) != this.sitesHaveBit) {
          // For map
          toRaw(this.leaflet.markerClusterGroupCamp).removeLayer(toRaw(currMarker));
          // For Table
          toRaw(currMarker).options.tableData.display = false;
          continue;
        }

        // if (this.compareBitmaps(currentBitmaps.campgndLandIs, this.campgndLandIsBit) != this.campgndLandIsBit) {
        //   // For map
        //   toRaw(this.leaflet.markerClusterGroupCamp).removeLayer(toRaw(currMarker));
        //   // For Table
        //   toRaw(currMarker).options.tableData.display = false;
        //   continue;
        // }

        // For map
        toRaw(this.leaflet.markerClusterGroupCamp).addLayer(toRaw(currMarker));
        // For Table
        toRaw(currMarker).options.tableData.display = true;
      }
    },
    compareBitmaps(markerBitmap, uiBitmap) {
      let result = markerBitmap & uiBitmap;
      return result;
    },
    textbox_query() {
      let textboxVal = $('#textbox').val();
      this.fetchOpenStreetData(textboxVal, (data) => {
        this.searchTextboxResults = [];
        this.searchMarkerTextboxResults = [];
        if (this.landCampVisibleArr[0] == true) {
          this.queryMarkers('State Park', this.stateParkMarkers, textboxVal);
        }
        if (this.landCampVisibleArr[1] == true) {
          this.queryMarkers('State Forest', this.stateForestMarkers, textboxVal);
        }
        if (this.landCampVisibleArr[2] == true) {
          this.queryMarkers('State Recreation Area', this.stateRecMarkers, textboxVal);
        }
        if (this.landCampVisibleArr[3] == true) {
          this.queryMarkers('State Wayside', this.stateWayMarkers, textboxVal);
        }
        if (this.landCampVisibleArr[4] == true) {
          this.queryMarkers('National Park', this.nationalParkMarkers, textboxVal);
        }
        if (this.landCampVisibleArr[5] == true) {
          this.queryMarkers('(Campground/Campsite)', this.stateParkCampMarkers, textboxVal);
          this.queryMarkers('(Campground/Campsite)', this.stateForestCampMarkers, textboxVal);
          this.queryMarkers('(Campground/Campsite)', this.nationalForestCampMarkers, textboxVal);
          this.queryMarkers('(Campground/Campsite)', this.countyCampMarkers, textboxVal);
          this.queryMarkers('(Campground/Campsite)', this.nationalGuardCampMarkers, textboxVal);
          this.queryMarkers('(Campground/Campsite)', this.koaCampMarkers, textboxVal);
          this.queryMarkers('(Campground/Campsite)', this.engineersCampMarkers, textboxVal);
          this.queryMarkers('(Campground/Campsite)', this.privateCampMarkers, textboxVal);
        }

        for (let element in data) {
          this.searchTextboxResults.push({
            display_name: data[element].display_name, lat: data[element].lat,
            lon: data[element].lon, type: "OSM", marker: null
          });
        }
        this.infoSlideActive("search_content_div");
      });
    },
    infoSlideActive(toSetActive) {
      $("#sidebar").removeClass("collapsed");
      $("#marker_content_div").removeClass("active");
      $("#marker_content_click_div").removeClass("active");
      $("#search_content_div").removeClass("active");
      $("#home").removeClass("active");
      $("#UI").removeClass("active");
      $("#search_result_content_div").removeClass("active");
      $("#modes").removeClass("active");
      $(`#${toSetActive}`).addClass("active");
      $(`.sidebar-content`).scrollTop(0);
    },
    flyTo(coords) {
      toRaw(this.leaflet.map).flyTo(coords, 10);
    },
    toggleUI() {
      const $UI = $('#UI');

      const $button = $('#UI_btn');
      const $search = $('#search');

      const currentClass = $button.attr('class');
      const hasDownClass = currentClass.includes('point_down');

      if (hasDownClass) {
        $button.addClass('point_up').removeClass('point_down');
        $search.addClass('searchDisplay').removeClass('searchNone');
      } else {
        $button.addClass('point_down').removeClass('point_up');
        $search.addClass('searchNone').removeClass('searchDisplay');
      }
    },
    queryMarkers(concatTag1, arr, textboxVal) {
      for (let element in arr) {
        let currMarker = toRaw(arr[element]);
        let markerValue = toRaw(currMarker).options.tableData.landName.toLowerCase();
        // let markerValue = currMarker._id.toLowerCase();
        let textboxValue = textboxVal.toLowerCase();

        if (markerValue.includes(textboxValue)) {
          this.searchMarkerTextboxResults.push({
            display_name: `${currMarker.options.tableData.landName} ${concatTag1}`, lat:
              currMarker._latlng.lat, lon: currMarker._latlng.lng, type: "Marker"
            , marker: currMarker
          });
          continue;
        }

        let textboxValArr = textboxValue.split(" ");
        for (let part in textboxValArr) {
          let currVal = textboxValArr[part];
          if (markerValue.includes(currVal)) {
            this.searchMarkerTextboxResults.push({
              display_name: `${currMarker.options.tableData.landName} ${concatTag1}`, lat:
                currMarker._latlng.lat, lon: currMarker._latlng.lng, type: "Marker"
              , marker: currMarker
            });
            break;
          }
        }

        // if (currMarker._id.toLowerCase() == textboxVal.toLowerCase() ||
        //   `${currMarker._id.toLowerCase()} ${concatTag1}` == textboxVal.toLowerCase() ||
        //   `${currMarker._id.toLowerCase()} ${concatTag2}` == textboxVal.toLowerCase()) {
        //   this.searchMarkerTextboxResults.push({
        //     display_name: `${currMarker._id} ${concatTag1}`, lat:
        //       currMarker._latlng.lat, lon: currMarker._latlng.lng, type: "Marker"
        //     , marker: currMarker
        //   });
        // }
      }
    },
    toggleDisplayTable() {
      this.displayTable = !this.displayTable;
    },
    landsLayerCheckedUpdate(value) {
      if (value == true) {
        toRaw(this.leaflet.map).addLayer(toRaw(this.leaflet.markerClusterGroupLand));
        $("#state_control_header").css("display", "");
        $(".marker_check_state").css("display", "");
        this.landCampVisibleArr[0] = true;
        this.landCampVisibleArr[1] = true;
        this.landCampVisibleArr[2] = true;
        this.landCampVisibleArr[3] = true;
        this.landCampVisibleArr[4] = true;
        for (let element in this.stateForestMarkers) {
          let marker = this.stateForestMarkers[element];
          toRaw(marker).options.tableData.display = true;
        }
        for (let element in this.stateParkMarkers) {
          let marker = this.stateParkMarkers[element];
          toRaw(marker).options.tableData.display = true;
        }
        for (let element in this.nationalParkMarkers) {
          let marker = this.nationalParkMarkers[element];
          toRaw(marker).options.tableData.display = true;
        }
        for (let element in this.stateRecMarkers) {
          let marker = this.stateRecMarkers[element];
          toRaw(marker).options.tableData.display = true;
        }
        for (let element in this.stateWayMarkers) {
          let marker = this.stateWayMarkers[element];
          toRaw(marker).options.tableData.display = true;
        }
      } else {
        toRaw(this.leaflet.map).removeLayer(toRaw(this.leaflet.markerClusterGroupLand));
        $("#state_control_header").css("display", "none");
        $(".marker_check_state").css("display", "none");
        this.landCampVisibleArr[0] = false;
        this.landCampVisibleArr[1] = false;
        this.landCampVisibleArr[2] = false;
        this.landCampVisibleArr[3] = false;
        this.landCampVisibleArr[4] = false;
        for (let element in this.stateForestMarkers) {
          let marker = this.stateForestMarkers[element];
          toRaw(marker).options.tableData.display = false;
        }
        for (let element in this.stateParkMarkers) {
          let marker = this.stateParkMarkers[element];
          toRaw(marker).options.tableData.display = false;
        }
        for (let element in this.nationalParkMarkers) {
          let marker = this.nationalParkMarkers[element];
          toRaw(marker).options.tableData.display = false;
        }
        for (let element in this.stateRecMarkers) {
          let marker = this.stateRecMarkers[element];
          toRaw(marker).options.tableData.display = false;
        }
        for (let element in this.stateWayMarkers) {
          let marker = this.stateWayMarkers[element];
          toRaw(marker).options.tableData.display = false;
        }
      }
    },
    campgroundsLayerCheckedUpdate(value) {
      if (value == true) {
        if (this.stateParkCampMarkers.length == 0) {
          $('.loading_slate').css("display", '');
          this.plotCampsiteMarkers('../../data/campground_split_webserve.json', () => {
            this.campgroundLayerAddAndUpdate();
            setTimeout(() => {
              $('.loading_slate').css("display", 'none');
            }, 1000);
          });
        } else {
          this.campgroundLayerAddAndUpdate();
          this.updateVisibleMarkers();
        }
      } else {
        this.campgroundLayerRemoveAndUpdate();
      }
    },
    campgroundLayerAddAndUpdate() {
      toRaw(this.leaflet.map).addLayer(toRaw(this.leaflet.markerClusterGroupCamp));
      $("#camp_control_header").css("display", "");
      $(".marker_check_camp").css("display", "");
      this.landCampVisibleArr[5] = true;
      for (let element in this.stateParkCampMarkers) {
        let marker = this.stateParkCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
      for (let element in this.stateForestCampMarkers) {
        let marker = this.stateForestCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
      for (let element in this.nationalForestCampMarkers) {
        let marker = this.nationalForestCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
      for (let element in this.privateCampMarkers) {
        let marker = this.privateCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
      for (let element in this.nationalGuardCampMarkers) {
        let marker = this.nationalGuardCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
      for (let element in this.countyCampMarkers) {
        let marker = this.countyCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
      for (let element in this.koaCampMarkers) {
        let marker = this.koaCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
      for (let element in this.engineersCampMarkers) {
        let marker = this.engineersCampMarkers[element];
        toRaw(marker).options.tableData.display = true;
      }
    },
    campgroundLayerRemoveAndUpdate() {
      toRaw(this.leaflet.map).removeLayer(toRaw(this.leaflet.markerClusterGroupCamp));
      $("#camp_control_header").css("display", "none");
      $(".marker_check_camp").css("display", "none");
      this.landCampVisibleArr[5] = false;
      for (let element in this.stateParkCampMarkers) {
        let marker = this.stateParkCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
      for (let element in this.stateForestCampMarkers) {
        let marker = this.stateForestCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
      for (let element in this.nationalForestCampMarkers) {
        let marker = this.nationalForestCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
      for (let element in this.privateCampMarkers) {
        let marker = this.privateCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
      for (let element in this.nationalGuardCampMarkers) {
        let marker = this.nationalGuardCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
      for (let element in this.countyCampMarkers) {
        let marker = this.countyCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
      for (let element in this.koaCampMarkers) {
        let marker = this.koaCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
      for (let element in this.engineersCampMarkers) {
        let marker = this.engineersCampMarkers[element];
        toRaw(marker).options.tableData.display = false;
      }
    },
    updateStateLandsVisible(emitArr) {
      let parkType = emitArr[0];
      let checkMarkCondition = emitArr[1];
      if (checkMarkCondition == true) {
        switch (parkType) {
          case "Park":
            this.turnOn(toRaw(this.stateParkMarkers))
            this.landCampVisibleArr[0] = true;
            break;
          case "Forest":
            this.turnOn(toRaw(this.stateForestMarkers))
            this.landCampVisibleArr[1] = true;
            break;
          case "Rec":
            this.turnOn(toRaw(this.stateRecMarkers))
            this.landCampVisibleArr[2] = true;
            break;
          case "Wayside":
            this.turnOn(toRaw(this.stateWayMarkers))
            this.landCampVisibleArr[3] = true;
            break;
          default:
            // default is NPS
            this.turnOn(toRaw(this.nationalParkMarkers));
            this.landCampVisibleArr[4] = true;
            break;
        }
      }
      else {
        switch (parkType) {
          case "Park":
            this.turnOff(toRaw(this.stateParkMarkers))
            this.landCampVisibleArr[0] = false;
            break;
          case "Forest":
            this.turnOff(toRaw(this.stateForestMarkers))
            this.landCampVisibleArr[1] = false;
            break;
          case "Rec":
            this.turnOff(toRaw(this.stateRecMarkers))
            this.landCampVisibleArr[2] = false;
            break;
          case "Wayside":
            this.turnOff(toRaw(this.stateWayMarkers))
            this.landCampVisibleArr[3] = false;
            break;
          default:
            // default is NPS
            this.turnOff(toRaw(this.nationalParkMarkers));
            this.landCampVisibleArr[4] = false;
            break;
        }
      }
    },
    updateCampTypeVis(emitArr) {
      // state, county, private, forest
      let campType = emitArr[0];
      let checkMarkCondition = emitArr[1];

      if (checkMarkCondition == true) {
        switch (campType) {
          case "State Park":
            this.turnOnCamp(toRaw(this.stateParkCampMarkers))
            this.campTypeSpeVis[0] = true;
            break;
          case "State Forest":
            this.turnOnCamp(toRaw(this.stateForestCampMarkers))
            this.campTypeSpeVis[1] = true;
            break;
          case "National Forest":
            this.turnOnCamp(toRaw(this.nationalForestCampMarkers))
            this.campTypeSpeVis[2] = true;
            break;
          case "County Park":
            this.turnOnCamp(toRaw(this.countyCampMarkers))
            this.campTypeSpeVis[3] = true;
            break;
          case "Private":
            this.turnOnCamp(toRaw(this.privateCampMarkers));
            this.campTypeSpeVis[4] = true;
            break;
          case "KOA":
            this.turnOnCamp(toRaw(this.koaCampMarkers));
            this.campTypeSpeVis[5] = true;
            break;
          case "Corps of Engineers":
            this.turnOnCamp(toRaw(this.engineersCampMarkers));
            this.campTypeSpeVis[6] = true;
            break;
          default:
            // default is national guard
            this.turnOnCamp(toRaw(this.nationalGuardCampMarkers));
            this.campTypeSpeVis[7] = true;
            break;
        }
      }
      else {
        switch (campType) {
          case "State Park":
            this.turnOffCamp(toRaw(this.stateParkCampMarkers))
            this.campTypeSpeVis[0] = false;
            break;
          case "State Forest":
            this.turnOffCamp(toRaw(this.stateForestCampMarkers))
            this.campTypeSpeVis[1] = false;
            break;
          case "National Forest":
            this.turnOffCamp(toRaw(this.nationalForestCampMarkers))
            this.campTypeSpeVis[2] = false;
            break;
          case "County Park":
            this.turnOffCamp(toRaw(this.countyCampMarkers))
            this.campTypeSpeVis[3] = false;
            break;
          case "Private":
            this.turnOffCamp(toRaw(this.privateCampMarkers));
            this.campTypeSpeVis[4] = false;
            break;
          case "KOA":
            this.turnOffCamp(toRaw(this.koaCampMarkers));
            this.campTypeSpeVis[5] = false;
            break;
          case "Corps of Engineers":
            this.turnOffCamp(toRaw(this.engineersCampMarkers));
            this.campTypeSpeVis[6] = false;
            break;
          default:
            // default is national guard
            this.turnOffCamp(toRaw(this.nationalGuardCampMarkers));
            this.campTypeSpeVis[7] = false;
            break;
        }
      }



      // if (checkMarkCondition == true) {
      //   this.turnOnCamp(toRaw(this.campMarkers), campType);
      // }
      // else {
      //   this.turnOffCamp(toRaw(this.campMarkers), campType);
      // }

    },
    updateUIBitmap(emitArr) {
      let bitToUpdate = emitArr[0];
      let value = emitArr[1];
      switch (bitToUpdate) {
        case "proTours":
          this.proToursBit += value;
          break;
        case "thingsToSee":
          this.thingsToSeeBit += value;
          break;
        case "campSeason":
          this.campSeasonBit += value;
          break;
        case "campsiteTypes":
          this.campsiteTypesBit += value;
          break;
        case "bathFacilities":
          this.bathFacilitiesBit += value;
          break;
        case "lodging":
          this.lodgingBit += value;
          break;
        case "trails":
          this.trailsBit += value;
          break;
        case "recFacilities":
          this.recFacilitiesBit += value;
          break;
        case "rentals":
          this.rentalsBit += value;
          break;
        default:
          // Default case will be winterRec bit
          this.winterRecBit += value;
          break;
      }
      this.updateVisibleMarkers();
    },
    updateCampUIBitmap(emitArr) {
      let bitToUpdate = emitArr[0];
      let value = emitArr[1];
      switch (bitToUpdate) {
        case "campSpecs":
          this.campSpecsBit += value;
          break;
        case "hookup":
          this.hookupBit += value;
          break;
        case "sitesHave":
          this.sitesHaveBit += value;
          break;
        // default:
        //   // default is campgndLandIsBit
        //   this.campgndLandIsBit += value;
        //   break;
      }
      this.updateVisibleMarkers();
    },
    panToandUpdate(coords) {
      this.flyTo(coords);
      this.leaflet.currCoords = coords;
      // if (type == 'search') {
        this.updateSearchClickMarker(coords);
      // }
    },
    closeClickableWindow() {
      if ($("#home").hasClass("active") == true) {
        $("#home_close").click();
      }
      if ($("#UI").hasClass("active") == true) {
        $("#UI_close").click();
      }
      if ($("#modes").hasClass("active") == true) {
        $("#modes_close").click();
      }
    }
  }
};
</script>
<template>
  <mapTableOverlay @tableMapAction="flyTo($event)" @tableVis="toggleDisplayTable($event)"
    :tableData="[this.stateParkMarkers, this.stateForestMarkers, this.stateRecMarkers, this.stateWayMarkers, this.nationalParkMarkers, this.stateParkCampMarkers, this.stateForestCampMarkers, this.nationalForestCampMarkers, this.countyCampMarkers, this.privateCampMarkers, this.koaCampMarkers, this.engineersCampMarkers, this.nationalGuardCampMarkers]"
    v-if="displayTable"></mapTableOverlay>
  <mapLoadingAnimation></mapLoadingAnimation>

  <div id="leafletmap" class="sidebar-map"></div>

  <mapSidebar @landsLayerChecked="landsLayerCheckedUpdate($event)"
    @campgroundsLayerChecked="campgroundsLayerCheckedUpdate($event)" @stateLandsChecked="updateStateLandsVisible($event)"
    :forecastData="this.forecastData" @updateBit="updateUIBitmap($event)" @updateCampUIBitmap="updateCampUIBitmap($event)"
    @updateCampTypeVis="$event => updateCampTypeVis($event)" @updateVisibleMarkers="updateVisibleMarkers()"
    @fly="flyTo($event)" @panToandUpdate="panToandUpdate($event)"
    :searchMarkerTextboxResults="this.searchMarkerTextboxResults" :searchTextboxResults="this.searchTextboxResults">
  </mapSidebar>


  <button id="flyBtn">Zoom</button>

  <div id="search" class="search-box animation_down">
    <div class="search_table_map_nav">
      <div style="display: flex;">
        <input id="textbox" type="text" placeholder="e.g. Gooseberry Falls">
      <button id="submit_btn" class="button" v-on:click=textbox_query>Submit</button>
      </div>
      
      <div style="width: 15px;"></div>
      <button id="toggle_table_btn" class="button" @click="toggleDisplayTable">List Filtered
        Results</button>
    </div>
  </div>
</template>
<style>

#leafletmap {
  width: 100%;
  height: 100vh;
  z-index: 0;
}

#flyBtn {
  position: absolute;
  right: 60px;
  bottom: 25px;
  background-color: white;
  border-radius: 5px;
  padding: 5px;
  border: solid 2px darkgray;
}

#flyBtn:hover {
  background-color: rgb(235, 235, 235); 
  cursor: pointer; 
}

#submit_btn {
  height: 40px;
  background-color: var(--main_color_1);
}

#submit_btn:hover {
  background-color: var(--main_color_2);
}

#toggle_table_btn {
  background-color: var(--main_color_1);
}

#toggle_table_btn:hover {
  background-color: var(--main_color_2);
}

.search_table_map_nav {
  display: flex; 
  flex-direction: column;
}

.search-box {
  position: absolute;
  top: 80px;
  left: 100px;
  /* display: flex;  */
}
#textbox {
  min-width: 100px;
  height: 40px;
}

.circle {
  background-clip: padding-box;
  border-radius: 50%;
  background-color: rgba(255, 247, 230, 0.8);
  border: solid black 1px;
  width: 40px;
  height: 40px;
  line-height: 31px;
  text-align: center;
}

.circle span {
  position: absolute;
  top: 1px;
  left: 14px;

}

#tent_img {
  height: 22px;
  width: 25px;
  opacity: 0.5;
}
</style>
