<script>
import Footer from './footer.vue'
import Home_Links from './home_links.vue'
import Camper_Anim from './camper_animation.vue'

export default {
    emits: ['executeMap'],
    props: ["appYear"],
    data() {
        return {
            counter: 0
        }
    },
    components: {
        Footer,
        Home_Links, 
        Camper_Anim
    },
    created() {
        $('html').css({
            overflow: 'hidden',
            height: '100%'
        });
    },
    methods: {
        goToMap() {
            this.$emit('executeMap', 'map');
        },
        scrollToNext() {
            $('#layer_2_text_div').addClass("layer_2_animation_appear");
            $("#layer_2_img1_div").addClass("ly_2_img_div_animation");
            $("#layer_2_img2_div").addClass("ly_2_img_div_animation");
            setTimeout(this.removeSplitRock, 2000);
            $('html').css({
                overflowY: 'auto',
                height: 'auto'
            });
        },
        removeSplitRock() {
            $('#image').css('display', 'none');
            $('#overlay').css('display', 'none');
        },
        checkLoad() {
            this.counter++;
            if (this.counter == 4) {
                $("#preloader_screen").css("display", "none");
                $('#image').addClass('image_animation');
                $('#overlay').addClass('overlay_animation')
                setTimeout(this.scrollToNext, 5000);
            }
        }
    }
}

</script>
<template>
    <div class="grid-x">
        <div class="large-12 medium-12 small-12 cell" id="home_main">
            <div class="loading_container" id="preloader_screen">
                <div class="loader_container">
                    <div class="spinner"></div>
                    <p class="loading_text">Please Wait...</p>
                </div>
            </div>

            <img class="" id="image" @load="this.checkLoad()"
                src="../../images/Split-Rock-Lighthouse-without-island-min.png" alt="Split Rock Lighhouse">
            <img class="" id="overlay" @load="this.checkLoad()" src="../../images/split-rock-island-min.png"
                alt="Split Rock Lighthouse Overlay">

            <div id="layer_2">
                <div class="grid-x">
                    <div class="large-6 medium-12 small-12 cell text_content">

                        <div id="layer_2_text_div">

                            <Camper_Anim></Camper_Anim>

                            <div style="height: 220px;"></div>

                            <h1 id="header_format">Welcome to Map-MN</h1>
                            <p class="layer_2_text">Here we are dedicated to gathering mapping resources and information
                                about Minnesota.
                                Below
                                you will find information and links to trusted websites that provide the most up to date
                                information
                                about Minnesota managed lands such as State Parks, State Forests, National Forests ,
                                State Recreational
                                Properties, State Waysides and National Park System managed lands all located in the
                                amazing state of
                                Minnesota! Below you will also find static maps you can either print out or request for
                                your next
                                Minnesota adventure.
                            </p>
                            <p class="layer_2_text">
                                If you really feel adventerous try out the interactive map in which you can navigate to
                                by either clicking the button below or clicking "MAP" on the navigation bar at the top
                                of the page.
                                There you will find all the Minnesota state lands above marked out as well as hundreds
                                of campgrounds
                                which you can filter through to find the perfect location for your next adventure.
                                However, please note
                                at this time the information displayed is regularly being updated. Recommendations and
                                suggestions on how
                                the information is inaccurate would be greatly appreciated. If you wish to share any
                                suggestions
                                please click the "FORM" tab in the navigation bar and fill out that form.
                            </p>
                            <button id="map-btn" class="button" @click="goToMap()">To Map</button>
                            <div class="buffer"></div>
                        </div>
                    </div>
                    <div class="large-6 medium-12 small-12 cell img_content">
                        <div id="layer_2_img1_div"><img id="ly_2_img_1" @load="this.checkLoad()"
                                src="../../images/mn_forest.jpg" alt="Minnesota Forest Trees">
                        </div>
                        <div id="layer_2_img2_div"><img id="ly_2_img_2" @load="this.checkLoad()"
                                src="../../images/tettgouche_lens.jpg" alt="Tettgouche State Park In Camera Lens">
                        </div>
                    </div>
                </div>
                <Home_Links style="text-align: left; "></Home_Links>
            </div>
            <Footer :year="this.appYear"></Footer>
        </div>
    </div>
</template>
<style>
#header_format {
    font-size: 3rem;
    color: #ffdd80;
    font-family: "Arial Black", Gadget, sans-serif;
    text-shadow: 0px 0px 0 rgb(238, 210, 132),
        0px 1px 0 rgb(224, 196, 118),
        0px 2px 0 rgb(211, 183, 105),
        0px 3px 0 rgb(197, 169, 91),
        0px 4px 0 rgb(184, 156, 78),
        0px 5px 0 rgb(170, 142, 64),
        0px 6px 0 rgb(157, 129, 51),
        0px 7px 0 rgb(144, 116, 38),
        0px 8px 7px rgba(0, 0, 0, 0.53),
        0px 8px 1px rgba(0, 0, 0, 0.5),
        0px 0px 7px rgba(0, 0, 0, .2);
    padding-top: 10px;
    padding-bottom: 10px;
}

#image {
    position: fixed;
    z-index: 200;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    /* transition: transform 1s ease-in-out; */

}

.image_animation {
    animation: zoom 7s linear 1 normal forwards;
    -webkit-animation: zoom 7s linear 1 normal forwards;
    -moz-animation: zoom 7s linear 1 normal forwards;
    -ms-animation: zoom 7s linear 1 normal forwards;
    -o-animation: zoom 7s linear 1 normal forwards;
}

@keyframes zoom {
    0% {
        transform: scale(100%);
        opacity: 1;
    }

    50% {
        transform: scale(110%);
        opacity: 1;
    }

    100% {
        transform: scale(120%);
        opacity: 0;
    }
}

#overlay {
    position: fixed;
    z-index: 200;
    top: 50px;
    left: 0px;
    width: 100%;
    height: 100%;
    /* object-fit: cover; */
    object-fit: fill;

}

.overlay_animation {
    animation: slideToRight 7s linear 1 normal forwards;
    -webkit-animation: slideToRight 7s linear 1 normal forwards;
    -moz-animation: slideToRight 7s linear 1 normal forwards;
    -ms-animation: slideToRight 7s linear 1 normal forwards;
    -o-animation: slideToRight 7s linear 1 normal forwards;
}

@keyframes slideToRight {
    0% {
        transform: translateX(0%) scale(100%);
        opacity: 1;
    }

    50% {
        transform: translateX(25%) scale(110%);
        opacity: 1;
    }

    100% {
        opacity: 0;
        transform: translateX(50%) scale(120%);
    }
}

#layer_2 {
    z-index: 1;
    padding-top: 100px;
    object-fit: cover;
    width: 100%;
    height: fit-content;
    text-align: center;
    background: rgb(67, 82, 165);
    background: linear-gradient(180deg, rgba(67, 82, 165, 1) 0%, rgba(157, 183, 237, 1) 35%, rgba(255, 247, 230, 1) 100%);
}

#layer_2 .text_content {
    padding: 20px;
}

.img_content {
    height: fit-content;
}


#layer_2_img1_div {
    height: fit-content;
    width: 100%;
    transform: rotate(8deg);
    background-color: white;
    border: outset #808080 5px;
    overflow: hidden;
    margin-left: 20px; 
    margin-top: -50px; 
}

#layer_2_img2_div {
    height: fit-content;
    transform: rotate(-8deg);
    margin-left: 100px;
    background-color: white;
    border: outset #808080 5px;
    border-style: outset;
    overflow: hidden;
    margin-top: -150px;
}

.ly_2_img_div_animation {
    /* animation: fade-in 5s ease-in-out 1 normal forwards; */
}

@keyframes fade-in {
    0% {
        opacity: 0;
    }

    100% {
        opacity: 1;
    }
}

.layer_2_img {
    height: fit-content;
    margin-top: 1000px;
    transform: scale(110%);
}

#ly_2_img_1 {
    width: 100%;
    animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -webkit-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -moz-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -ms-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -o-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
}

#ly_2_img_1:hover {
    animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -webkit-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -moz-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -ms-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -o-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
}

#ly_2_img_2 {
    width: 100%;
    animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -webkit-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -moz-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -ms-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
    -o-animation: slightZoomOut 0.5s ease-in-out 1 normal forwards;
}

#ly_2_img_2:hover {
    animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -webkit-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -moz-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -ms-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
    -o-animation: slightZoomIn 0.5s ease-in-out 1 normal forwards;
}

@keyframes slightZoomIn {
    0% {
        transform: scale(102%);
    }

    100% {
        transform: scale(120%);
    }
}

@keyframes slightZoomOut {
    0% {
        transform: scale(120%);
    }

    100% {
        transform: scale(102%);
    }
}

#layer_2_text_div {
    opacity: 0;
    font-size: 1.2rem;
    height: fit-content;
}

.layer_2_animation_appear {
    animation: scaleAndMoveApp 3s ease-in-out 1 normal forwards;
    -webkit-animation: scaleAndMoveApp 3s ease-in-out 1 normal forwards;
    -moz-animation: scaleAndMoveApp 3s ease-in-out 1 normal forwards;
    -ms-animation: scaleAndMoveApp 3s ease-in-out 1 normal forwards;
    -o-animation: scaleAndMoveApp 3s ease-in-out 1 normal forwards;
}

.layer_2_animation_disappear {
    animation: scaleAndMoveDis 3s ease-in-out 1 normal forwards;
    -webkit-animation: scaleAndMoveDis 3s ease-in-out 1 normal forwards;
    -moz-animation: scaleAndMoveDis 3s ease-in-out 1 normal forwards;
    -ms-animation: scaleAndMoveDis 3s ease-in-out 1 normal forwards;
    -o-animation: scaleAndMoveDis 3s ease-in-out 1 normal forwards;
}


@keyframes scaleAndMoveApp {
    0% {
        transform: scale(0.8) translateY(100px);
        opacity: 0;
    }

    100% {
        transform: scale(1) translateY(0px);
        opacity: 1;
    }
}

@keyframes scaleAndMoveDis {
    0% {
        transform: scale(1) translateY(0px);
        opacity: 1;
    }

    100% {
        transform: scale(0.8) translateY(100px);
        opacity: 0;
    }

}

#map-btn {
    background-color: var(--main_color_1);
}

#map-btn:hover {
    background-color: var(--main_color_2);
}

.loading_container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 1000;
    background-color: rgba(200, 200, 200, 0)
}

.loader_container {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: linear-gradient(-45deg, var(--main_color_1), var(--main_color_2), var(--main_color_3), var(--main_color_5));
    background-size: 400% 400%;
    animation: gradient 15s ease infinite;
}

@keyframes gradient {
    0% {
        background-position: 0% 50%;
    }

    50% {
        background-position: 100% 50%;
    }

    100% {
        background-position: 0% 50%;
    }
}

.loading_text {
    font-size: 1.2rem;
    font-weight: bold;
    position: absolute;
    padding-top: 250px;
}

.spinner {
    position: relative;
    width: 200px;
    height: 200px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.spinner::before,
.spinner::after {
    border: 15.6px solid #4352a5;
    border-radius: 50%;
    position: absolute;
    content: '';
    display: block;
}

.spinner::before {
    width: 100.2px;
    height: 100.2px;
    border-bottom-color: transparent;
    border-left-color: transparent;
    animation: spinner-1o3y8q 1.0s infinite linear reverse;
}

.spinner::after {
    animation: spinner-1o3y8q 0.75s infinite linear;
    height: 150px;
    width: 150px;
    border-right-color: transparent;
    border-top-color: transparent;
}

@keyframes spinner-1o3y8q {
    to {
        transform: rotate(360deg);
    }
}

.buffer {
    height: 50px;
}
</style>