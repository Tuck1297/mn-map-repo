<script>
import { toRaw } from 'vue';


export default {
    props: ['tableData'],
    emits: ['tableVis', 'tableMapAction'],
    methods: {
        webTabOpen(url) {
            if (url[0] == 'h') {
                window.open(url, '_blank');
            } else {
                window.open(`https://www.dnr.state.mn.us${url}`, '_blank')
            }
        },
        closeTable() {
            this.$emit('tableVis', false);
        },
        goToMarker(marker) {
            toRaw(marker).fire('click');
            this.$emit('tableVis', false);
            this.$emit('tableMapAction', marker._latlng);
        },
        determineClass(parkType) {
            let [category, type] = parkType.split(':');
            // Campground Type: 
            // Park Type: 
            switch (category) {
                case "Park Type":
                    switch (type) {
                        case " State Park":
                            return 'park'
                        case " State Forest":
                            return 'forest'
                        case " State Wayside":
                            return 'wayside'
                        case " State Recreation Area":
                            return 'recreation'
                        case " National Scenic Riverway":
                            return 'nps'
                        case " National Park":
                            return 'nps'
                        case " National Forest":
                            return 'nps'
                        case " National Monument":
                            return 'nps'
                        case " National Scenic Trail":
                            return 'nps'
                        case " National River & Recreation Area":
                            return 'nps'
                        case " National Monument":
                            return 'nps'
                        default:
                            break;
                    }
                default:
                    // Campground Type: 
                    return 'camp'
            }
        },
        showData() {
            //console.log(this.tableData)

        }
    }
}

</script>
<template>
    <div class="table_opac"></div>
    <div class="table_div_center">
        <div class="table_format">
            <div class="grid-x">
                <div class="large-1 medium-2 small-0 cell"></div>
                <div class="large-10 medium-8 small-12 cell table_div">
                    <button id="table_close_btn" @click="closeTable"><img src="../../images/x.png" alt="X"></button>
                    <div class="show-for-small-only" style="margin-top: 50px; ">
                        <div class="legend_box_small park">
                            <p>State Park </p>
                        </div>
                        <div class="legend_box_small forest">
                            <p>State Forest </p>
                        </div>
                        <div class="legend_box_small wayside">
                            <p>State Wayside </p>
                        </div>
                        <div class="legend_box_small recreation">
                            <p>State Recreation Area </p>
                        </div>
                        <div class="legend_box_small nps">
                            <p>National Park Service </p>
                        </div>
                        <div class="legend_box_small camp">
                            <p>Campsite </p>
                        </div>
                    </div>
                    <div class="show-for-medium-only" style="margin-top: 50px; ">
                        <div class="legend_box_small park">
                            <p>State Park </p>
                        </div>
                        <div class="legend_box_small forest">
                            <p>State Forest </p>
                        </div>
                        <div class="legend_box_small wayside">
                            <p>State Wayside </p>
                        </div>
                        <div class="legend_box_small recreation">
                            <p>State Recreation Area </p>
                        </div>
                        <div class="legend_box_small nps">
                            <p>National Park Service </p>
                        </div>
                        <div class="legend_box_small camp">
                            <p>Campsite </p>
                        </div>
                    </div>
                    <div class="legend_wrapper show-for-large" style="margin-top: 50px;">
                        <div class="grid-x">
                            <div class="large-4 medium-4 small-4 cell">
                                <div class="legend">
                                    <p>State Park: </p>
                                    <div class="legend_box park"></div>
                                </div>
                                <div class="legend">
                                    <p>State Forest: </p>
                                    <div class="legend_box forest"></div>
                                </div>
                            </div>
                            <div class="large-4 medium-4 small-4 cell">
                                <div class="legend">
                                    <p>State Wayside: </p>
                                    <div class="legend_box wayside"></div>
                                </div>
                                <div class="legend">
                                    <p>State Rec Area: </p>
                                    <div class="legend_box recreation"></div>
                                </div>
                            </div>
                            <div class="large-4 medium-4 small-4 cell">
                                <div class="legend">
                                    <p>National Park Service: </p>
                                    <div class="legend_box nps"></div>
                                </div>
                                <div class="legend">
                                    <p>Campsite: </p>
                                    <div class="legend_box camp"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- <button class="button" @click="showData">Button</button> -->
                    <div id="table_wrapper">
                        <table id="info_data_table">
                            <tr>
                                <th>
                                    <h4>Map Information</h4>
                                </th>
                            </tr>
                            <tr v-for="arr in tableData">
                                <div v-for="(element, index) in arr"
                                    :class="determineClass(element.options.tableData.landType)">
                                    <div class="table_data_div" v-if="element.options.tableData.display">
                                        <span class="clickable" @click="goToMarker(element)">
                                            {{ element.options.tableData.landName }}
                                            {{ element.options.tableData.landType.split(":")[1] }}
                                        </span>
                                        <br>
                                        <span v-if="element.options.tableData.url != 'UNKNOWN'" class="clickable"
                                            @click="webTabOpen(element.options.tableData.url)">Website</span>
                                        <br>
                                        <span class="nonclickable">{{
                                            element.options.tableData.address.split("<br>")[0]
                                        }}</span>
                                    </div>
                                </div>
                            </tr>


                            <!-- <tr v-for="(element, index) in tableData" :class="(index % 2 === 0) ? 'even' : 'odd'"> -->
                            <!-- <td><a :href="`https://www.dnr.state.mn.us${element[6]}`" target="_blank">{{ element[5] }}</a></td> -->
                            <!-- <td v-if="element[7]" class="clickable" @click="webRedirect(element[6])">{{ element[5] }}</td> -->
                            <!-- <td v-if="element[7]">{{ element[2] }}</td> -->
                            <!-- <td v-if="element[7]">{{ element[1] }}</td> -->
                            <!-- <td v-if="element.display" > -->
                            <!-- <span class="clickable">{{ element.landName }} {{ element.landType.split(":")[1] }}</span> -->
                            <!-- <br> -->
                            <!-- <span class="clickable" @click="webTabOpen(element.url)">Website</span> -->
                            <!-- <br> -->
                            <!-- <span class="nonclickable">{{ element.address.split(":")[1] }}</span> -->
                            <!-- </td> -->
                            <!-- </tr> -->
                        </table>
                    </div>
                </div>
                <div class="large-1 medium-2 small-0 cell"></div>
            </div>
        </div>
    </div>
</template>
<style>
.table_opac {
    position: absolute;
    /* top: 50%;  */
    /* left: 50%; */
    z-index: 200;
    width: 100%;
    height: 100%;
    background-color: var(--main_color_4);
    opacity: 0.5;
}

.table_div_center {
    position: absolute;
    top: 200px;
    left: 0;
    right: 0;
    margin-left: auto;
    margin-right: auto;
    z-index: 201;
}

.table_div {
    background-color: var(--main_color_4);
    border-radius: 5px;
    border: 4px double black;
    padding: 10px;
    height: 500px;
    overflow-y: scroll;
}

.table_data_div,
th {
    border: solid 1px black;
}

#table_wrapper {
    display: flex;
    justify-content: center;
}

#info_data_table {
    table-layout: fixed;
    width: 75%;
    text-align: center;
}

#info_data_table th {
    background-color: darkgray;
}

.clickable {
    cursor: pointer;
    color: #1779ba;
    padding: 5px;
    font-size: 1.2rem;
}

.clickable:hover {
    color: white;
    background-color: var(--main_color_2);
    border-radius: 10px;
}

.nonclickable {
    padding: 5px;
    font-size: 1.2rem;
}

.park {
    background-color: var(--main_color_5);
}

.wayside {
    background-color: var(--main_color_9)
}

.forest {
    background-color: var(--main_color_3);
}

.recreation {
    background-color: var(--main_color_6);
}

.nps {
    background-color: var(--main_color_8)
}

.camp {
    background-color: var(--main_color_7)
}

.legend {
    display:flex; 
    justify-content: center; 
    width: 250px;
    font-weight: bold; 
    font-size: 1.2rem; 
}

.legend_wrapper {
    display: flex;
    justify-content: center;
}

.legend_box {
    margin-left: 10px;
    margin-right: 10px;
    width: 20px;
    height: 20px;
}

.legend_box_small {
    width: 100%;
    height: 30px;
    margin: 10px;
    text-align: center;
    font-size: 1.2rem;
    font-weight: bold;
}

#table_close_btn {
    /* float: right; */
    position: absolute;
    padding-bottom: 10px;
}

#table_close_btn:hover {
    filter: brightness(0) invert(1);
    cursor: pointer;
}
</style>