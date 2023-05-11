<script>
import { toRaw } from 'vue';

export default {
    props: ['forecastData', 'searchMarkerTextboxResults', 'searchTextboxResults'],
    emits: ['landsLayerChecked', 'campgroundsLayerChecked', 'stateLandsChecked', 'updateBit', 'updateCampUIBitmap', 'updateVisibleMarkers', 'fly', 'panToandUpdate', 'updateCampTypeVis'],
    data() {
        return {
            layerControls: [
                { label: "Minnesota State Lands", id: "StateLnd", arrIndex: 0 },
                { label: "Minnesota Campgrounds", id: "CampGnd", arrIndex: 1 }
            ],
            layerArr: [true, false],
            stateUIControls: [
                { label: "State Parks", id: "State", arrIndex: 1 },
                { label: "State Recreation", id: "Rec", arrIndex: 2 },
                { label: "State Wayside", id: 'Way', arrIndex: 3 },
                { label: "NPS Properties", id: "NPS", arrIndex: 4 },
                { label: "State Forests", id: "Forest", arrIndex: 5 },
                { label: "Nature Programs", id: "nature", arrIndex: 6 },
                { label: "Tours", id: "tour", arrIndex: 7 },
                { label: "Waterfalls", id: "fall", arrIndex: 8 },
                { label: "Fire Tower", id: "fire", arrIndex: 9 },
                { label: "Historic Site", id: "history", arrIndex: 10 },
                { label: "Winter Camping", id: "wint", arrIndex: 11 },
                { label: "Spring Camping", id: "spr", arrIndex: 12 },
                { label: "Summer Camping", id: "sum", arrIndex: 13 },
                { label: "Fall Camping", id: "fall", arrIndex: 14 },
                { label: "Drive-In Campsite", id: "driveCamp", arrIndex: 15 },
                { label: "Accessible Drive-In Campsite", id: "accDriveCamp", arrIndex: 16 },
                { label: "Pull Through Campsite", id: "pullThrough", arrIndex: 17 },
                { label: "30 Amp Hookup", id: "amp30", arrIndex: 18 },
                { label: "50 Amp Hookup", id: "amp50", arrIndex: 19 },
                { label: "Horse Campsite", id: "horseCamp", arrIndex: 20 },
                { label: "Cart-In Campsite", id: "cartCamp", arrIndex: 21 },
                { label: "Walk-In Campsite", id: "walkCamp", arrIndex: 22 },
                { label: "Walk-In Campsite (over 1/2 mile)", id: "halfMiWalkCamp", arrIndex: 23 },
                { label: "Canoe/Kayak/Boat Access Campsite", id: "boatAccessCamp", arrIndex: 24 },
                { label: "Group Camp", id: "groupCamp", arrIndex: 25 },
                { label: "Showers", id: "shower", arrIndex: 26 },
                { label: "Accessible Showers", id: "accShowers", arrIndex: 27 },
                { label: "Flush Toilets", id: "flushToilet", arrIndex: 28 },
                { label: "Accessible Flush Toilets", id: "accFlushToilet", arrIndex: 29 },
                { label: "RV Dump Station", id: "dumpStation", arrIndex: 30 },
                { label: "Camper Cabin", id: "campCabin", arrIndex: 31 },
                { label: "Accessible Camper Cabin", id: "accCampCabin", arrIndex: 32 },
                { label: "Guesthouse or other cabin", id: "otherCabin", arrIndex: 33 },
                { label: "Group Center", id: "groupCenter", arrIndex: 34 },
                { label: "Yurt", id: "yurt", arrIndex: 35 },
                { label: "Accessible Yurt", id: "accYurt", arrIndex: 36 },
                { label: "Tipi or Wall Tent", id: "tipiWT", arrIndex: 37 },
                { label: "Hiking Trails", id: "hiking", arrIndex: 38 },
                { label: "Paved Biking Trails", id: "pavedBike", arrIndex: 39 },
                { label: "Mountain Biking Trails", id: "mtnBike", arrIndex: 40 },
                { label: "Horse Trails", id: "horseTrails", arrIndex: 41 },
                { label: "Off-Highway Vehicle Trails", id: "offHighTrails", arrIndex: 42 },
                { label: "Vistor Center", id: "vCenter", arrIndex: 43 },
                { label: "Picnic Shelter", id: "picShelter", arrIndex: 44 },
                { label: "Accessible Picnic Shelter", id: "accpicShelter", arrIndex: 45 },
                { label: "Swimming Beach", id: "swimBeach", arrIndex: 46 },
                { label: "Accessible Swimming Beach", id: "accSwimBeach", arrIndex: 47 },
                { label: "Playground", id: "playGnd", arrIndex: 48 },
                { label: "Accessible Playground", id: "accPlayGnd", arrIndex: 49 },
                { label: "Volleyball Court", id: "volleyCourt", arrIndex: 50 },
                { label: "Athletic fields", id: "athField", arrIndex: 51 },
                { label: "Electric Vehicle Charging", id: "evCharge", arrIndex: 52 },
                { label: "Rock Climbing", id: "rockClimb", arrIndex: 53 },
                { label: "Fishing Pier", id: "fishPier", arrIndex: 54 },
                { label: "Drive-In Boat Access", id: "driveBoat", arrIndex: 55 },
                { label: "Walk-In Boat Access", id: "walkBoat", arrIndex: 56 },
                { label: "Dock", id: "dock", arrIndex: 57 },
                { label: "Canoe Rental", id: "canoeRent", arrIndex: 58 },
                { label: "Kayak Rental", id: "kayakRent", arrIndex: 59 },
                { label: "Stand-Up Paddleboard Rental", id: "paddleboardRent", arrIndex: 60 },
                { label: "Boat Rental", id: "boatRent", arrIndex: 61 },
                { label: "Snowshoe Rental", id: "snowshoeRent", arrIndex: 62 },
                { label: "Cross-Country Ski Rental", id: "skiRent", arrIndex: 63 },
                { label: "Warming Shelter", id: "warmShelter", arrIndex: 64 },
                { label: "Snowmobile Trails", id: "snowmobile", arrIndex: 65 },
                { label: "Groomed Cross-Country Ski Trails", id: "crossCountryTrails", arrIndex: 66 },
                { label: "Groomed Skate Ski Trails", id: "skateTrails", arrIndex: 67 },
                { label: "Groomed Snowshoe Trails", id: "snowshoeTrials", arrIndex: 68 },
                { label: "Groomed Fatbike Trails", id: "fatbikeTrails", arrIndex: 69 },
                { label: "Plowed or Packed Hiking Trails", id: "plowPackHikeTrails", arrIndex: 70 },
                { label: "Sledding Hill", id: "sledHill", arrIndex: 71 },
            ],
            stateUIArr: [null, true, true, true, true, true, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false],
            campUIControls: [
                { label: "Tents Allowed", id: "tntAlw", arrIndex: 0 },
                { label: "Rvs Allowed", id: "RvsAlw", arrIndex: 1 },
                { label: "Wall Tent", id: "walltnt", arrIndex: 2 },
                { label: "Yurt", id: "", arrIndex: 3 },
                { label: "Tipis", id: "", arrIndex: 4 },
                { label: "Cabin", id: "", arrIndex: 5 },
                { label: "Horse Campground", id: "", arrIndex: 6 },
                { label: "Handicap Accessible", id: "", arrIndex: 7 },
                { label: "Cart-In Site", id: "", arrIndex: 8 },
                { label: "Backpack Site", id: "", arrIndex: 9 },
                { label: "Walk-In Site", id: "", arrIndex: 10 },
                { label: "Bike-In Site", id: "", arrIndex: 11 },
                { label: "Boat-In Site", id: "", arrIndex: 12 },
                { label: "Hike", id: "", arrIndex: 13 },
                { label: "Big Rigs Accepted", id: "", arrIndex: 14 },
                { label: "Electric Hookup", id: "", arrIndex: 15 },
                { label: "Electric-Water Hookup", id: "", arrIndex: 16 },
                { label: "Full Hookup", id: "", arrIndex: 17 },
                { label: "30/50 AMP", id: "", arrIndex: 18 },
                { label: "Pull Through Site", id: "", arrIndex: 19 },
                { label: "Table", id: "", arrIndex: 20 },
                { label: "Grill", id: "", arrIndex: 21 },
                { label: "Water", id: "", arrIndex: 22 },
                { label: "Toilet", id: "", arrIndex: 23 },
                { label: "Shower", id: "", arrIndex: 24 },
                { label: "Dump Station", id: "", arrIndex: 25 },
                { label: "Internet/Wi-Fi", id: "", arrIndex: 26 },
                { label: "State Park Campgrounds", id: "", arrIndex: 27 },
                { label: "State Forest Campgrounds", id: "", arrIndex: 28 },
                { label: "National Forest Campgrounds", id: "", arrIndex: 29 },
                { label: "County Park Campgrounds", id: "", arrIndex: 30 },
                { label: "Private Campgrounds", id: "", arrIndex: 31 },
                { label: "KOA Campgrounds", id: "", arrIndex: 32 },
                { label: "Corps of Engineers Campsites", id: "", arrIndex: 33 },
                { label: "Minnesota National Guard", id: "", arrIndex: 34 }
            ],
            campUIArr: [false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, true, true, true, true, true, true, true, true]
        }
    },
    computed: {
        statepChecked() {
            return this.stateUIArr[1];
        },
        stateRecChecked() {
            return this.stateUIArr[2];
        },
        stateWayChecked() {
            return this.stateUIArr[3];
        },
        NPSChecked() {
            return this.stateUIArr[4];
        },
        stateForestChecked() {
            return this.stateUIArr[5];
        },
        natureProgramsChecked() {
            return this.stateUIArr[6];
        },
        toursChecked() {
            return this.stateUIArr[7];
        },
        waterfallChecked() {
            return this.stateUIArr[8];
        },
        firetowerChecked() {
            return this.stateUIArr[9];
        },
        historicSiteChecked() {
            return this.stateUIArr[10];
        },
        winterCampingChecked() {
            return this.stateUIArr[11];
        },
        springCampingChecked() {
            return this.stateUIArr[12];
        },
        summerCampingChecked() {
            return this.stateUIArr[13];
        },
        fallCampingChecked() {
            return this.stateUIArr[14];
        },
        drive_in_campsiteChecked() {
            return this.stateUIArr[15];
        },
        acc_driveCampsiteChecked() {
            return this.stateUIArr[16];
        },
        pull_throughCampsiteChecked() {
            return this.stateUIArr[17];
        },
        amp30HookupChecked() {
            return this.stateUIArr[18];
        },
        amp50HookupChecked() {
            return this.stateUIArr[19];
        },
        horseCampsiteChecked() {
            return this.stateUIArr[20];
        },
        cartCampsiteChecked() {
            return this.stateUIArr[21];
        },
        walkCampsiteChecked() {
            return this.stateUIArr[22];
        },
        walkCampsiteOverHalfMiChecked() {
            return this.stateUIArr[23];
        },
        watercraftCampsiteChecked() {
            return this.stateUIArr[24];
        },
        groupCampChecked() {
            return this.stateUIArr[25];
        },
        showerChecked() {
            return this.stateUIArr[26];
        },
        accShowerChecked() {
            return this.stateUIArr[27];
        },
        flushToiletChecked() {
            return this.stateUIArr[28];
        },
        accFlushtoiletChecked() {
            return this.stateUIArr[29];
        },
        DumpstationChecked() {
            return this.stateUIArr[30];
        },
        campCabinChecked() {
            return this.stateUIArr[31];
        },
        accCamperCabinChecked() {
            return this.stateUIArr[32];
        },
        guesthouseOtherCabinChecked() {
            return this.stateUIArr[33];
        },
        groupCenterChecked() {
            return this.stateUIArr[34];
        },
        yurtChecked() {
            return this.stateUIArr[35];
        },
        accYurtChecked() {
            return this.stateUIArr[36];
        },
        tipiWallTentChecked() {
            return this.stateUIArr[37];
        },
        hikingTrailChecked() {
            return this.stateUIArr[38];
        },
        pavedBikeTrailChecked() {
            return this.stateUIArr[39];
        },
        mtnBikeTrailChecked() {
            return this.stateUIArr[40];
        },
        horseTrailChecked() {
            return this.stateUIArr[41];
        },
        offhighwayTrailChecked() {
            return this.stateUIArr[42];
        },
        visitorCenterChecked() {
            return this.stateUIArr[43];
        },
        picnicShelterChecked() {
            return this.stateUIArr[44];
        },
        accPicnicShelterChecked() {
            return this.stateUIArr[45];
        },
        swimBeachChecked() {
            return this.stateUIArr[46];
        },
        accSwimBeachChecked() {
            return this.stateUIArr[47];
        },
        playgroundChecked() {
            return this.stateUIArr[48];
        },
        accPlaygroundChecked() {
            return this.stateUIArr[49];
        },
        volleyballCourtChecked() {
            return this.stateUIArr[50];
        },
        athFieldsChecked() {
            return this.stateUIArr[51];
        },
        evChargingChecked() {
            return this.stateUIArr[52];
        },
        rockClimbingChecked() {
            return this.stateUIArr[53];
        },
        accFishPierChecked() {
            return this.stateUIArr[54];
        },
        driveBoatAccessChecked() {
            return this.stateUIArr[55];
        },
        carryBoatAccessChecked() {
            return this.stateUIArr[56];
        },
        dockChecked() {
            return this.stateUIArr[57];
        },
        canoeRentChecked() {
            return this.stateUIArr[58];
        },
        kayakRentChecked() {
            return this.stateUIArr[59];
        },
        paddleboardRentChecked() {
            return this.stateUIArr[60];
        },
        boatRentChecked() {
            return this.stateUIArr[61];
        },
        snowshoeRentChecked() {
            return this.stateUIArr[62];
        },
        ccSkiRentChecked() {
            return this.stateUIArr[63];
        },
        warmingShelterChecked() {
            return this.stateUIArr[64];
        },
        snowmobileTrailChecked() {
            return this.stateUIArr[65];
        },
        ccSkiTrailChecked() {
            return this.stateUIArr[66];
        },
        skateSkiTrailChecked() {
            return this.stateUIArr[67];
        },
        snowshoeTrailChecked() {
            return this.stateUIArr[68];
        },
        fatBikeTrailChecked() {
            return this.stateUIArr[69];
        },
        plowPackHikingTrailChecked() {
            return this.stateUIArr[70];
        },
        sledHillChecked() {
            return this.stateUIArr[71];
        },
        landsLayerChecked() {
            return this.layerArr[0];
        },
        campgroundsLayerChecked() {
            return this.layerArr[1];
        },
        tentChecked() {
            return this.campUIArr[0];
        },
        rvChecked() {
            return this.campUIArr[1];
        },
        wallTentChecked() {
            return this.campUIArr[2];
        },
        yurtChecked() {
            return this.campUIArr[3];
        },
        tipiChecked() {
            return this.campUIArr[4];
        },
        cabinChecked() {
            return this.campUIArr[5];
        },
        horseCampgroundChecked() {
            return this.campUIArr[6];
        },
        handiCapChecked() {
            return this.campUIArr[7];
        },
        cartInChecked() {
            return this.campUIArr[8];
        },
        backpackChecked() {
            return this.campUIArr[9];
        },
        walkInChecked() {
            return this.campUIArr[10];
        },
        bikeSiteChecked() {
            return this.campUIArr[11];
        },
        boatSiteChecked() {
            return this.campUIArr[12];
        },
        hikeChecked() {
            return this.campUIArr[13];
        },
        bigRigAccChecked() {
            return this.campUIArr[14];
        },
        electricChecked() {
            return this.campUIArr[15];
        },
        electricWaterChecked() {
            return this.campUIArr[16];
        },
        fullHookChecked() {
            return this.campUIArr[17];
        },
        Amp50Checked() {
            return this.campUIArr[18];
        },
        pullThruChecked() {
            return this.campUIArr[19];
        },
        tableChecked() {
            return this.campUIArr[20];
        },
        grillChecked() {
            return this.campUIArr[21];
        },
        waterChecked() {
            return this.campUIArr[22];
        },
        toiletsChecked() {
            return this.campUIArr[23];
        },
        showersChecked() {
            return this.campUIArr[24];
        },
        dumpStationChecked() {
            return this.campUIArr[25];
        },
        internetChecked() {
            return this.campUIArr[26];
        },
        stateParkCampgroundChecked() {
            return this.campUIArr[27];
        },
        stateForestCampgroundChecked() {
            return this.campUIArr[28];
        },
        nationalForestCampgroundChecked() {
            return this.campUIArr[29];
        },
        countyChecked() {
            return this.campUIArr[30];
        },
        privateChecked() {
            return this.campUIArr[31];
        },
        koaChecked() {
            return this.campUIArr[32];
        },
        engineersChecked() {
            return this.campUIArr[33];
        },
        otherChecked() {
            return this.campUIArr[34];
        }
    },
    watch: {
        statepChecked(value) {
            this.$emit('stateLandsChecked', ['Park', value]);
        },
        NPSChecked(value) {
            this.$emit('stateLandsChecked', ['NPS', value]);
        },
        stateRecChecked(value) {
            this.$emit('stateLandsChecked', ['Rec', value]);
        },
        stateWayChecked(value) {
            this.$emit('stateLandsChecked', ['Wayside', value]);
        },
        stateForestChecked(value) {
            this.$emit('stateLandsChecked', ['Forest', value]);
        },
        natureProgramsChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['proTours', 2]);
            } else {
                this.$emit('updateBit', ['proTours', -2]);
            }
        },
        toursChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['proTours', 1]);
            } else {
                this.$emit('updateBit', ['proTours', -1]);
            }

        },
        waterfallChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['thingsToSee', 4]);
            } else {
                this.$emit('updateBit', ['thingsToSee', -4]);
            }

        },
        firetowerChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['thingsToSee', 2]);
            } else {
                this.$emit('updateBit', ['thingsToSee', -2]);
            }

        },
        historicSiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['thingsToSee', 1]);
            } else {
                this.$emit('updateBit', ['thingsToSee', -1]);
            }

        },
        winterCampingChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campSeason', 8]);
            } else {
                this.$emit('updateBit', ['campSeason', -8]);
            }

        },
        springCampingChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campSeason', 4]);
            } else {
                this.$emit('updateBit', ['campSeason', -4]);
            }

        },
        summerCampingChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campSeason', 2]);
            } else {
                this.$emit('updateBit', ['campSeason', -2]);
            }

        },
        fallCampingChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campSeason', 1]);
            } else {
                this.$emit('updateBit', ['campSeason', -1]);
            }

        },
        drive_in_campsiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 1024]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -1024]);
            }

        },
        acc_driveCampsiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 512]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -512]);
            }

        },
        pull_throughCampsiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 256]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -256]);
            }

        },
        amp30HookupChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 128]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -128]);
            }

        },
        amp50HookupChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 64]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -64]);
            }

        },
        horseCampsiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 32]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -32]);
            }

        },
        cartCampsiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 16]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -16]);
            }

        },
        walkCampsiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 8]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -8]);
            }

        },
        walkCampsiteOverHalfMiChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 4]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -4]);
            }

        },
        watercraftCampsiteChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 2]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -2]);
            }

        },
        groupCampChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['campsiteTypes', 1]);
            } else {
                this.$emit('updateBit', ['campsiteTypes', -1]);
            }

        },
        showerChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['bathFacilities', 16]);
            } else {
                this.$emit('updateBit', ['bathFacilities', -16]);
            }

        },
        accShowerChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['bathFacilities', 8]);
            } else {
                this.$emit('updateBit', ['bathFacilities', -8]);
            }

        },
        flushToiletChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['bathFacilities', 4]);
            } else {
                this.$emit('updateBit', ['bathFacilities', -4]);
            }

        },
        accFlushtoiletChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['bathFacilities', 2]);
            } else {
                this.$emit('updateBit', ['bathFacilities', -2]);
            }

        },
        DumpstationChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['bathFacilities', 1]);
            } else {
                this.$emit('updateBit', ['bathFacilities', -1]);
            }

        },
        campCabinChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['lodging', 64]);
            } else {
                this.$emit('updateBit', ['lodging', -64]);
            }

        },
        accCamperCabinChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['lodging', 32]);
            } else {
                this.$emit('updateBit', ['lodging', -32]);;
            }

        },
        guesthouseOtherCabinChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['lodging', 16]);
            } else {
                this.$emit('updateBit', ['lodging', -16]);;
            }

        },
        groupCenterChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['lodging', 8]);
            } else {
                this.$emit('updateBit', ['lodging', -8]);
            }

        },
        yurtChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['lodging', 4]);
            } else {
                this.$emit('updateBit', ['lodging', -4]);
            }

        },
        accYurtChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['lodging', 2]);
            } else {
                this.$emit('updateBit', ['lodging', -2]);
            }

        },
        tipiWallTentChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['lodging', 1]);
            } else {
                this.$emit('updateBit', ['lodging', -1]);
            }

        },
        hikingTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['trails', 16]);
            } else {
                this.$emit('updateBit', ['trails', -16]);
            }

        },
        pavedBikeTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['trails', 8]);
            } else {
                this.$emit('updateBit', ['trails', -8]);
            }

        },
        mtnBikeTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['trails', 4]);
            } else {
                this.$emit('updateBit', ['trails', -4]);
            }

        },
        horseTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['trails', 2]);
            } else {
                this.$emit('updateBit', ['trails', -2]);
            }

        },
        offhighwayTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['trails', 1]);
            } else {
                this.$emit('updateBit', ['trails', -1]);
            }

        },
        visitorCenterChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 16384]);
            } else {
                this.$emit('updateBit', ['recFacilities', -16384]);
            }

        },
        picnicShelterChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 8192]);
            } else {
                this.$emit('updateBit', ['recFacilities', -8192]);
            }

        },
        accPicnicShelterChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 4096]);
            } else {
                this.$emit('updateBit', ['recFacilities', -4096]);
            }

        },
        swimBeachChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 2048]);
            } else {
                this.$emit('updateBit', ['recFacilities', -2048]);
            }

        },
        accSwimBeachChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 1024]);
            } else {
                this.$emit('updateBit', ['recFacilities', -1024]);
            }

        },
        playgroundChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 512]);
            } else {
                this.$emit('updateBit', ['recFacilities', -512]);
            }

        },
        accPlaygroundChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 256]);
            } else {
                this.$emit('updateBit', ['recFacilities', -256]);
            }

        },
        volleyballCourtChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 128]);
            } else {
                this.$emit('updateBit', ['recFacilities', -128]);
            }

        },
        athFieldsChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 64]);
            } else {
                this.$emit('updateBit', ['recFacilities', -64]);
            }

        },
        evChargingChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 32]);
            } else {
                this.$emit('updateBit', ['recFacilities', -32]);
            }

        },
        rockClimbingChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 16]);
            } else {
                this.$emit('updateBit', ['recFacilities', -16]);
            }

        },
        accFishPierChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 8]);
            } else {
                this.$emit('updateBit', ['recFacilities', -8]);
            }

        },
        driveBoatAccessChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 4]);
            } else {
                this.$emit('updateBit', ['recFacilities', -4]);
            }

        },
        carryBoatAccessChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 2]);
            } else {
                this.$emit('updateBit', ['recFacilities', -2]);
            }

        },
        dockChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['recFacilities', 1]);
            } else {
                this.$emit('updateBit', ['recFacilities', -1]);
            }

        },
        canoeRentChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['rentals', 32]);
            } else {
                this.$emit('updateBit', ['rentals', -32]);
            }

        },
        kayakRentChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['rentals', 16]);
            } else {
                this.$emit('updateBit', ['rentals', -16]);
            }

        },
        paddleboardRentChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['rentals', 8]);
            } else {
                this.$emit('updateBit', ['rentals', -8]);
            }

        },
        boatRentChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['rentals', 4]);
            } else {
                this.$emit('updateBit', ['rentals', -4]);
            }

        },
        snowshoeRentChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['rentals', 2]);
            } else {
                this.$emit('updateBit', ['rentals', -2]);
            }

        },
        ccSkiRentChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['rentals', 1]);
            } else {
                this.$emit('updateBit', ['rentals', -1]);
            }

        },
        warmingShelterChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 128]);
            } else {
                this.$emit('updateBit', ['winterRec', -128]);
            }

        },
        snowmobileTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 64]);
            } else {
                this.$emit('updateBit', ['winterRec', -64]);
            }

        },
        ccSkiTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 32]);
            } else {
                this.$emit('updateBit', ['winterRec', -32]);
            }

        },
        skateSkiTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 16]);
            } else {
                this.$emit('updateBit', ['winterRec', -16]);
            }

        },
        snowshoeTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 8]);
            } else {
                this.$emit('updateBit', ['winterRec', -8]);
            }

        },
        fatBikeTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 4]);
            } else {
                this.$emit('updateBit', ['winterRec', -4]);
            }

        },
        plowPackHikingTrailChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 2]);
            } else {
                this.$emit('updateBit', ['winterRec', -2]);
            }

        },
        sledHillChecked(value) {
            if (value == true) {
                this.$emit('updateBit', ['winterRec', 1]);
            } else {
                this.$emit('updateBit', ['winterRec', -1]);
            }

        },
        landsLayerChecked(value) {
            this.$emit('landsLayerChecked', value);
            if (value == false) {
                this.resetStateLandsControls();
            }
        },
        campgroundsLayerChecked(value) {
            this.$emit('campgroundsLayerChecked', value);
            if (value == false) {
                this.resetStateCampControls();
            }
        },

        tentChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 16384]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -16384])
            }
        },
        rvChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 8192]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -8192])
            }
        },
        wallTentChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 4096]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -4096])
            }
        },
        yurtChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 2048]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -2048])
            }
        },
        tipiChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 1024]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -1024])
            }
        },
        cabinChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 512]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -512])
            }
        },
        horseCampgroundChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 256]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -256])
            }
        },
        handiCapChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 128]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -128])
            }
        },
        cartInChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 64]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -64])
            }
        },
        backpackChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 32]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -32])
            }
        },
        walkInChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 16]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -16])
            }
        },
        bikeSiteChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 8]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -8])
            }
        },
        boatSiteChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 4]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -4])
            }
        },
        hikeChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 2]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -2])
            }
        },
        bigRigAccChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['campSpecs', 1]);
            } else {
                this.$emit('updateCampUIBitmap', ['campSpecs', -1])
            }
        },
        electricChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['hookup', 8]);
            } else {
                this.$emit('updateCampUIBitmap', ['hookup', -8])
            }
        },
        electricWaterChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['hookup', 4]);
            } else {
                this.$emit('updateCampUIBitmap', ['hookup', -4])
            }
        },
        fullHookChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['hookup', 2]);
            } else {
                this.$emit('updateCampUIBitmap', ['hookup', -2])
            }
        },
        Amp50Checked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['hookup', 1]);
            } else {
                this.$emit('updateCampUIBitmap', ['hookup', -1])
            }
        },
        pullThruChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 128]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -128])
            }
        },
        tableChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 64]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -64])
            }
        },
        grillChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 32]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -32])
            }
        },
        waterChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 16]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -16])
            }
        },
        toiletsChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 8]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -8])
            }
        },
        showersChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 4]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -4])
            }
        },
        dumpStationChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 2]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -2])
            }
        },
        internetChecked(value) {
            if (value == true) {
                this.$emit('updateCampUIBitmap', ['sitesHave', 1]);
            } else {
                this.$emit('updateCampUIBitmap', ['sitesHave', -1])
            }
        },
        stateParkCampgroundChecked(value) {
            this.$emit('updateCampTypeVis', ['State Park', value]);
        },
        stateForestCampgroundChecked(value) {
            this.$emit('updateCampTypeVis', ['State Forest', value]);
        },
        nationalForestCampgroundChecked(value) {
            this.$emit('updateCampTypeVis', ['National Forest', value]);
        },
        countyChecked(value) {
            this.$emit('updateCampTypeVis', ['County Park', value]);
        },
        privateChecked(value) {
            this.$emit('updateCampTypeVis', ['Private', value]);
        },
        koaChecked(value) {
            this.$emit('updateCampTypeVis', ['KOA', value]);
        },
        engineersChecked(value) {
            this.$emit('updateCampTypeVis', ['Corps of Engineers', value]);
        },
        otherChecked(value) {
            this.$emit('updateCampTypeVis', ['Minnesota National Guard', value]);
        }
    },
    methods: {
        backToResults() {
            this.infoSlideActive("search_content_div");
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
        resetUIControls() {
            this.resetStateLandsControls();
            this.resetStateCampControls();
            this.$emit('updateVisibleMarkers', 'update');

        },
        resetStateLandsControls() {
            this.stateUIArr = [null, true, true, true, true, true, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false];
        },
        resetStateCampControls() {
            this.campUIArr = [false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false, true, true, true, true, true, true, true, true];
        },
        showClickedInfo(coords, type, marker) {
            if (type == "OSM") {
                this.fetchOpenStreetDataFromLatLon(coords, (data) => {
                    let message = this.processAddressData(data);
                    $("#search_result_content").html(message);
                    this.infoSlideActive("search_result_content_div");
                    this.$emit('panToandUpdate', coords);
                })
            } else {
                toRaw(marker).fire('click');
                this.flyTo(coords);
                $("#backBtn").css("display", "");
            }

        },
        fetchOpenStreetData(val, callback) {
            fetch(`https://nominatim.openstreetmap.org/search?q='${val}, Minnesota'&format=json&limit=10&accept-language=en&countrycodes=us`)
                .then(response => response.json())
                .then(data => {
                    if (data.length == 0) {
                        console.log('Your requested query cannot be fulfilled at this time.');
                    }
                    callback(data)
                })
                .catch(error => {
                    console.log(error);
                    console.log('Your requested query cannot be fulfilled at this time.');
                });
        },
        fetchOpenStreetDataFromLatLon(coords, callback) {
            // coords is an array [lng, lat]
            let lat = coords.lat;
            let lng = coords.lng;
            fetch(`https://nominatim.openstreetmap.org/reverse?lat=${lat}&lon=${lng}&format=json`)
                .then(response => response.json())
                .then(data => { callback(data) })
                .catch(error => {
                    console.log(error);
                    console.log('Your requested query cannot be fulfilled at this time.');
                })
        },
        flyTo(coords) {
            this.$emit('fly', coords);
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
    }
}
</script>

<template>
    <div id="sidebar" class="sidebar collapsed" style="height: 75%; top: 120px; z-index: 100;">
        <!-- Nav tabs -->
        <div class="sidebar-tabs">
            <ul role="tablist">
                <li><a href="#home" role="tab"><img src="" class="map_img" data="../../images/home.png" alt="Home"></a></li>
                <li><a href="#UI" role="tab"><img src="" class="map_img" data="../../images/filter.png"
                            alt="Map Filters"></a></li>
                <li><a href="#modes" role="tab"><img style="width: 30px;" src="" class="map_img"
                            data="../../images/mode.png" alt="Mode"></a>
                </li>
                <!-- <li><a href="#marker_content_div">tabtemp</a></li> --> <!--For Testing Purposes-->
            </ul>
        </div>

        <!-- Tab panes -->
        <div class="sidebar-content">
            <div class="sidebar-pane" id="home">
                <h1 class="sidebar-header">
                    Home
                    <span id="home_close" class="sidebar-close"><img id="close_btn" src="" class="map_img"
                            data="../../images/x.png" alt="X"></span>
                </h1>
                <div class="buffer"><!--Buffer--></div>
                <h1 class="sub_content_header">Marker Legend</h1>
                <p>State Park Marker: <img class="marker_image map_img" src="" data="../../images/sp3_marker.png"
                        alt="State Park Marker"></p>
                <p>State Forest Marker: <img class="marker_image map_img" src="" data="../../images/sr3_marker.png"
                        alt="State Forest Marker">
                </p>
                <p>State Recreation Area Marker: <img class="marker_image map_img" src="" data="../../images/sw_marker.png"
                        alt="State Wayside Marker"></p>
                <p>National Park Lands Marker: <img class="marker_image map_img" src="" data="../../images/nps_marker.png"
                        alt="National Park Marker"></p>
                <p>Campsite Marker: <img class="marker_image map_img" src="" data="../../images/camp_marker.png"
                        alt="Campsite Marker">
                </p>
                <p>Clicked Location Marker: <img class="marker_image map_img" src="" data="../../images/curr_marker.png"
                        alt="Location Marker">
                </p>
                <p>State Lands Cluster Marker: <img class="cluster_marker_image map_img" src="" data="../../images/cluster-state-lands.JPG"
                    alt="State Cluster Marker"/>
                </p>
                <p>State Campground Cluster Marker: <img class="cluster_marker_image map_img" src="" data="../../images/cluster-camp.JPG" 
                    alt="Campground Cluster Marker"/>
                </p>
                <p>
                    <h1 class="sub_content_header">Navigating the Map</h1>
                    <p>
                        <h1 class="sub_content_header">Click</h1> 
                        <ul>
                            <li>Click and Drag: Allow you to move around the map.</li>
                            <li>Click a marker: Upon clicking a marker the sidebar will present additional information about the location that the 
                                marker represents. This includes Name, Highlights, Latitude and Longitude and Weather information. 
                            </li>
                            <li>Click on a cluster marker: The Cluster Markers are meant to give a simplified representation of the hundreds of markers
                                that can be displayed at one time. The number on the cluster marker represents how many markers are being represented by 
                                the cluster marker. If you click a cluster the map will zoom into that region and update the map to 
                                display a more detailed representation of that part of Minnesota. 
                            </li>
                            <li>Click anywhere else on the map. When clicking on the map and not dragging or clicking on a marker the current Location 
                                marker will update to that location. The sidebar will open displaying some basic information about your clicked location. 
                            </li>
                        </ul>
                    </p>
                    <p>
                        <h1 class="sub_content_header">Map Controls - bottom right corner</h1> 
                        <p>
                            Zoom: This button will zoom you in to the last clicked/interacted with marker. By default if no marker has been interacted with it will 
                            zoom into the center of the state. 
                        </p>
                        <p>
                            Layers: This is the button that has three squares layered on top of each other. Upon hovering over this button you will see two 
                            checkmark options. One is for "Satellite" and one is for "Street View". These are essentially different map offerings to provide you with 
                            different information upon zooming in or interacting with the map.   
                        </p>
                        <p>
                            (+) (-) Buttons: Like Google Maps these buttons give you a simple way of zooming in and out of the map. 
                        </p>
                    </p>
                </p>
                <p>
                    <h1 class="sub_content_header">Modes - <img class="map_img" src="" data="../../images/mode.png"
                        alt="Filter Icon"></h1>
                        <p>
                            This is the mode window where you will be presented with a few different 
                            collections of data that you can either enable or disable by using the selection 
                            of a single or all checkboxes. However, note that application performance will be 
                            reduced as more modes are being used side-by-side.     
                        </p>
                    </p>
                <p>
                    <h1 class="sub_content_header">Filters - <img class="map_img" src="" data="../../images/filter.png"
                        alt="Filter Icon"></h1>
                    <p>
                        Based on the mode that is currently checked this window it will 
                        display filters that only relate to the current collection of markers. 
                        This will allow one to find state lands and campgrounds that fulfill your
                        potential needs. Such questions one can answer with these filters
                        include determining which State Park have a waterfall and campsites with
                        electric hookup. Also on this page there is a  "RESET ALL Control" button that 
                        resets all the filters to their initial factory default. 
                    </p>    
                </p>
                <p>
                    <h1 class="sub_content_header">Table Display</h1>
                    <p>
                        Toward the middle of the map screen you will see a button labeled "List Filtered Results"
                        By clicking on this button you will be presented with a simplified version of the resulting data from 
                        any filters you clicked. Each column represents one marker on the map. By selecting the name of the 
                        Park-Forest/Campground you will be redirected to the map where the sidebar will display more detailed 
                        information about that entity. If you click "Website" a new page will open directing you to the current
                        website associated with that property. Please know that website links update and change all the time so 
                        if you discover a broken link please submit a form which you can access from the navigation bar. 
                    </p>
                </p>
                <p>
                    <h1 class="sub_content_header">Search Bar</h1>
                    <p>
                        To the top left of the map you will see a search bar. This search bar is specifically created to search our records
                        we have on file for Minnesota State Lands and Campgrounds. However, a basic function has been added to query any addresses 
                        or other locations in Minnesota. It is possible that there is a searched entity you are looking for that has not 
                        been cataloged yet. If this is the case, please submit a form indicating the addition of such information. This map 
                        is constantly being updated so any information you would like to see included will be integrated. 
                    </p>
                </p>
            </div>

            <div class="sidebar-pane" id="UI">
                <h1 class="sidebar-header">
                    Controls
                    <span id="UI_close" class="sidebar-close"><img id="close_btn" src="" class="map_img"
                            data="../../images/x.png" alt="X"></span>
                </h1>
                <!-- Buffer Element -->
                <div style="height: 15px;"></div>
                <button @click="resetUIControls" class="button">Reset All Controls</button>
                <!-- Buffer Element -->
                <div style="height: 15px;"></div>
                <h1 id="state_control_header" class="sub_content_header">Minnesota State Lands Controls</h1>
                <p v-for="element in stateUIControls" class="marker_check_state">
                    <input type="checkbox" :id=element.id v-model=stateUIArr[element.arrIndex]> <span>{{
                        element.label
                    }}</span>
                </p>
                <h1 id="camp_control_header" class="sub_content_header" style="display: none;">Minnesota Campgrounds
                    Controls
                </h1>
                <!-- Buffer Element -->
                <div style="height: 15px;"></div>
                <p v-for="element in campUIControls" class="marker_check_camp" style="display: none;">
                    <input type="checkbox" :id="element.id" v-model=campUIArr[element.arrIndex]> <span>{{
                        element.label
                    }}</span>
                </p>
            </div>

            <div class="sidebar-pane" id="modes">
                <h1 class="sidebar-header">
                    Map Modes
                    <span id="modes_close" class="sidebar-close"><img id="close_btn" src="" class="map_img"
                            data="../../images/x.png" alt="X"></span>
                </h1>
                <div style="height: 15px;"><!--Buffer--></div>
                <p v-for="element in layerControls">
                    <input type="checkbox" :id=element.id v-model=layerArr[element.arrIndex]> <span>{{
                        element.label
                    }}</span>
                </p>
            </div>

            <div class="sidebar-pane" id="marker_content_click_div">
                <h1 class="sidebar-header" id="marker_content_header">Map Information<span class="sidebar-close"><img
                            id="close_btn" src="" class="map_img" data="../../images/x.png" alt="X"></span></h1>
                <p id="marker_content_click"></p>
            </div>

            <div class="sidebar-pane" id="marker_content_div">
                <h1 class="sidebar-header" id="marker_content_header">Map Information<span class="sidebar-close"><img
                            id="close_btn" src="" class="map_img" data="../../images/x.png" alt="X"></span></h1>
                <p id="marker_content">
                <p class="info_header" id="header_link"><a href="https://www.dnr.state.mn.us/${currLand.URL}"
                        target="_blank">Hello</a></p>
                <p class="info_element" style="text-align: center;" id="estab_id"></p>
                <p class="info_element" style="text-align: center;" id="phone_id"></p>
                <p class="info_element" id="address_id"></p>
                <p class="info_element" id="type_id"></p>
                <p class="info_element" id="desc_id"></p>
                Highlights
                <ul id="highlights_id"></ul>
                <p class="info_element"></p>
                <div style="overflow-x: auto">
                    <table>
                        <tr>
                            <th></th>
                            <th>Decimal Coordinate</th>
                            <th>Degree Minutes Seconds (DMS)</th>
                        </tr>
                        <tr>
                            <th>Latitude</th>
                            <td id="lat_id"></td>
                            <td id="lat_dms"></td>
                        </tr>
                        <tr>
                            <th>Longitude</th>
                            <td id="lon_id"></td>
                            <td id="lon_dms"></td>
                        </tr>
                    </table>
                </div>
                <p class="info_element"></p>
                <p class="info_element">
                    7-Day Weather Forecast
                <div style="overflow-x: auto">
                    <table>
                        <tr>
                            <td v-for="element in forecastData">
                                <div class="weatherDiv">
                                    <p>{{ element.name }}</p>
                                    <img :src=element.icon alt="Weather Icon">
                                    <p>{{ element.temp }}</p>
                                    <p>{{ element.windData }}</p>
                                    <p>{{ element.shortForecast }}</p>
                                </div>
                            </td>
                        </tr>
                    </table>
                </div>
                </p>
                <p id="dnr_id"></p>
                <button class="button" id="backBtn" style="display: none;" v-on:click="backToResults">BACK</button>
                </p>
            </div>

            <div class="sidebar-pane" id="search_content_div">
                <h1 class="sidebar-header" id="search_content_header">Search Results<span class="sidebar-close"><img
                            id="close_btn" src="" class="map_img" data="../../images/x.png" alt="X"></span></h1>
                <h1 class="sub_content_header">Map Information Search Results: </h1>
                <p v-for="element in searchMarkerTextboxResults">
                    <button class="search_result_btn"
                        v-on:click="showClickedInfo({ lat: element.lat, lng: element.lon }, element.type, element.marker)">{{
                            element.display_name
                        }}</button>
                </p>
                <h1 class="sub_content_header">OpenStreetMap Search Results: </h1>
                <p v-for="element in searchTextboxResults">
                    <button class="search_result_btn"
                        v-on:click="showClickedInfo({ lat: element.lat, lng: element.lon }, element.type, element.marker)">{{
                            element.display_name
                        }}</button>
                </p>
            </div>

            <div class="sidebar-pane" id="search_result_content_div">
                <h1 class="sidebar-header" id="marker_content_header">Your Result<span class="sidebar-close"><img
                            id="close_btn" src="" class="map_img" data="../../images/x.png" alt="X"></span></h1>
                <p style="text-align: center;" id="search_result_content"></p>
                <button class="button" v-on:click="backToResults">BACK</button>
            </div>

        </div>
    </div>
</template>

<style>

#close_btn:hover {
    filter: brightness(0) invert(1);
}

.info_header {
    padding-top: 10px;
    padding-bottom: 15px;
    font-size: 1.2rem;
    margin-bottom: -5px;
    text-align: center;
    font-weight: bold;
    text-decoration: underline;
    text-decoration-color: #1779ba;
}

.info_element {
    padding-right: 5px;
    padding-bottom: 10px;
    margin-top: -5px;
    border-bottom: 1px solid black;
}

.search_result_btn {
    padding: 5px;
    margin-top: 5px;
    margin-bottom: 5px;
}

.search_result_btn:hover {
    background-color: #cccccc;
    border-radius: 15px;
    cursor: pointer;
    padding: 5px;
    margin-top: 5px;
    margin-bottom: 5px;
}

.weatherDiv {
    padding: 3px;
    border: solid 2px black;
    text-align: center;
    background-color:
        var(--main_color_4);
    min-height: 300px;
    min-width: 175px;
}

.weatherDiv img {
    height: 4rem;
    width: 4rem;
}

.sub_content_header {
    font-size: 1rem;
    text-decoration-line: underline;
    font-weight: 600;
}

.marker_legend {
    display: flex;
    justify-content: space-between;

}

.marker_image {
    width: 25px;
    height: 40px;
}

.cluster_marker_image {
    width: 40px; 
    height: 40px; 
    border-radius: 50%; 
}

td,
th {
    min-width: 100px;
    border: solid 1px black;
}

.buffer {
    height: 20px; 
}

</style>