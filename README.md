# Minnesota Interactive State Map Project

IMAGE WILL GO HERE!!!

This project is the initial concept and development behind the interactive map component for a community driven application where users can interacte and share their experiences throughout the state of Minnesota. The focus of this project initially was ensuring the development of a economical but feature driven interactive map where data can be queried and the map will reflect it. This concept project will be one of the main features of this application where users can apply filters to sort thorugh data relative to either the part of the the state they want to explore or a particular activity they are looking for. You can check out the beta release of this project showcasing the interactive map component [here](). The code in this repo is for review only.

---

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
## TODOS

- Wait for feedback...

# Extra Functionalities
- TODO: bounding box that displays a 10/20/50/100 mile radius around the marker that is currently clicked (searched/camp/state lands)
- TODO: Integrate air quality API: https://open-meteo.com/en/docs/air-quality-api
- TODO: Integrate Dark Sky API: Provides weather forecasts and historical weather data for a given location.
- TODO: Campground Reviews API: Allows you to access a database of campground reviews, photos, and information.

# When build national map
- USDA National Parks Service API: Provides information about national parks, including park details, campground information, and alerts.


## Tools and Resources

- Chosen Color Pallete with colormind.io    
    - Header and Footer = #366778
    - Navigation hover background = #41aaa8
    - Background Generation = #f2e7a8
    - Background generation = #d4a05d
    - Some other color use = #d8523b

- Images: 
    - Image by <a href="https://pixabay.com/users/clker-free-vector-images-3736/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=311280">Clker-Free-Vector-Images</a> from <a href="https://pixabay.com//?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=311280">Pixabay</a>

- Software: 
    - https://ngageoint.github.io/geopackage-viewer-js/
    - https://geojson.io/#map=12/45.01043/-93.64473/0/42

- Comments: 
    - Data to start with and path for this project...
        1. Start with State Parks, State Forests, and County Parks
        2. Eventually start logging Walmarts, private campsites, and private resorts (when begin getting traffic)
        <!-- 3. Then create interactive maps for each state park  -->
        4. Look into mapping all outlined hiking trails on state park maps
        5. Far down the road - integrate someone being able to create an account and save their saved preferences (want to visit - screen snapshot)
        6. Also have all markers to be filtered by certain types of conditions such as type of campsite if park has cabins and other stuff
        7. Make sure my website is entirely accessible
        8. Add privacy policy to make sure website is fully legal



current structure of camping bitmap: 
                campSpecsBit: 0,
                hookupBit: 0,
                sitesHaveBit: 0,
                campgndLandIsBit: 0,
camp specs:
    tent
    rv
    wall tent
    yurt
    tipis
    cabin
    horse
    handicap
    cart-in
    backpack
    walk-in
    bike
    paddle-in
    hike
    big rig accepted
hookups: 
    electric hookup
    electric-water hookup
    full hookup
    30/50 amp
sites have: 
    pull thru
    table
    grill
    water
    toilets
    showers
    dump
    internet/wifi
camp land type: 
    state park
    state forest
    county park
    private

## Other info: 

https://developer.here.com/documentation/geocoding-search-api/dev_guide/index.html
https://www.liedman.net/leaflet-control-geocoder/docs/
https://leafletjs.com/plugins.html#basemap-providers
https://www.wikiwand.com/en/List_of_county_and_regional_parks_in_Minnesota
https://gisdata.mn.gov/dataset?q=forest&sort=score+desc%2C+metadata_modified+desc&ext_bbox=&ext_prev_extent=-105.46875%2C39.90973623453719%2C-80.5078125%2C52.05249047600099
https://www.arcgis.com/home/webmap/viewer.html?featurecollection=https%3A%2F%2Fgis.mda.state.mn.us%2Farcgis%2Frest%2Fservices%2FMDA_Basemap%2FMDA_BasemapC%2FMapServer%3Ff%3Djson%26option%3Dfootprints&supportsProjection=true&supportsJSONP=true
https://gisdata.mn.gov/group
https://www.dnr.state.mn.us/mis/gis/index.html

https://geojson.io/#map=13/47.14222/-91.47949
https://www.convertcsv.com/csv-to-json.htm
https://www.fs.usda.gov/chippewa/
https://www.nps.gov/voya/index.htm
https://www.dnr.state.mn.us/state_forests/forest.html?id=sft00002#homepage
https://www.convertcsv.com/json-to-csv.htm
https://open-meteo.com/
https://ngageoint.github.io/geopackage-viewer-js/
https://gisdata.mn.gov/dataset/trans-state-park-trails-roads
http://www.uscampgrounds.info/

## Future Prospects
# Integrate the Following: 
    - scientific and Natual Area Units - gisdata.mn.gov/dataset
    - Minnesota Trails - gisdata.mn.gov/dataset 
    - Scenic Byways in Minnesota Resources - gisdata.mn.gov/dataset
    - Memorial Routes in Minnesota - gisdata.mn.gov/dataset
    - Rest Areas and MnDOT Facilities in Minnesota - gisdata.mn.gov/dataset
    - Walmarts
    - Add on to and update campgrounds in the state
    - Waterfalls <-- do this one next

https://cssgradient.io/
https://www.html-code-generator.com/css/3d-text-generator
https://app.haikei.app/
https://10015.io/tools/css-loader-generator

https://www.countyoffice.org/mn-campground/
https://www.exploreminnesota.com/places-to-stay/campgrounds#!grid~~~Featured~1
