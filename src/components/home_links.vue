<template>
    <div class="grid-x">
        <div class="large-12 medium-12 small-12 cell" style="text-align: center;">
            <div class="buffer show-for-small-only"></div>
            <div class="header_container">
                <h1 class="layer_3_header">Resources</h1>
            </div>
        </div>
        <div class="large-6 medium-6 small-0 cell">
            <div class="header_container">
                <h1 class="layer_3_header">Websites</h1>
            </div>
            <p class="layer_3_body">
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.dnr.state.mn.us/parkfinder/index.html">MN State Parks
                    and Recreational Areas</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.dnr.state.mn.us/state_forests/map.html">MN State
                    Forests</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.dnr.state.mn.us/state_parks/waysides/index.html">MN
                    State Waysides</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.fs.usda.gov/superior">Superior National Forest</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.fs.usda.gov/chippewa/">Chippewa National Forest</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.nps.gov/voya/index.htm">Voyagers National Park</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.fs.usda.gov/recarea/superior/recarea/?recid=84168">Boundary
                    Waters
                    Canoe Area</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.dnr.state.mn.us/wmas/index.html">MN Wildlife Mangement
                    Areas</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://511mn.org/rest-area/15/@-92.89839,45.60659,7?show=restAreas">MN
                    Rest
                    Areas</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.exploreminnesota.com/">Explore Minnesota</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank"
                    href="https://www.midwestliving.com/travel/minnesota/20-top-things-to-do-minnesota/?slide=164ac25b-7f03-4639-9f1c-3f6b6bd31fdd#164ac25b-7f03-4639-9f1c-3f6b6bd31fdd">Things
                    To Do In Minnesota</a>
            </p>
        </div>
        <!-- NOTE:  Add an image or some other content filler on larger screens where it will be hidden on smaller screens-->
        <div class="large-6 medium-6 small-0 cell">
            <div class="header_container">
                <h1 class="layer_3_header">Maps</h1>
            </div>
            <p class="layer_3_body">
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.dnr.state.mn.us/state_parks/map.html">State Park
                    Printable Map</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="../../images/mn_forest_map.jpg">State Forest Map</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.exploreminnesota.com/order-travel-guides">Minnesota
                    Highway Map (Order Online - FREE)</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.dot.state.mn.us/statemap/">Minnesota Highway Map (View
                    Online)</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank"
                    href="https://www.dot.state.mn.us/maps/gdma/data/maps/bicycle/state/statebikemap.pdf">Minnesota
                    Bicycle Map</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.supercoloring.com/coloring-pages/map-of-minnesota">Printable Minnesota
                    State Map Coloring Page</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="">Map-MN Printable Map ~Coming Soon~</a>
                <br>
                <img class="list_icon_img" src="../../images/icon.png" alt="List Icon">
                <a target="_blank" href="https://www.dot.mn.gov/restareas/images/511.jpg">MN Rest Areas
                    Map</a>
                <!-- <br> -->
                <!-- <a target="_blank" href=""></a> -->
                <!-- <br> -->
                <!-- <a target="_blank" href=""></a> -->
            </p>
        </div>
    </div>
</template>
<style>

.header_container {
    display: flex;
    justify-content: center;
}

.layer_3_header {
    background-color: rgba(255, 255, 255, 0.8);
    border-radius: 50%;
    padding: 20px;
    text-decoration: underline;
    border: 10px groove grey;
    transition: 0.3s;
    margin-top: 30px; 
}

.layer_3_header:hover {
    transform: scale(110%);
    transition: 0.3s;
    border: 10px grey groove;
    border-color: var(--main_color_1);
    background-color: var(--main_color_2);
    color: white;
}

.layer_3_body {
    padding: 10px;
    font-size: 1rem;
    background-color: white;
    margin: 10px;
    border-radius: 20px;
    border: ridge 5px grey;
}

.layer_3_body a {
    color: var(--main_color_1)
}

.layer_3_body a:active {
    color: var(--main_color_6);
}

.layer_3_body a:hover {
    color: var(--main_color_2)
}

.layer_3_body a:visited {
    color: purple;
}

</style>