<script>
import Footer from './footer.vue'
export default {
    props: ["appYear", "currentPage"],
    data() {
        return {
            box_clicked: false,
            footerYear: false,
            imageData: [
                { imageURL: '../../images/jQuery.png', imageALT: 'jQuery Logo', imageDescription: 'jQuery is a free and open-source library designed to simplify HTML DOM tree traversal and manipulation. This resource was used in getting conditions from UI controls and updating the crime tablebased on the map conditions.', imageIndex: 0, id: 'img-0' },
                { imageURL: '../../images/leaflet.png', imageALT: 'Leaflet Logo', imageDescription: 'Leaflet is a open-source library used to build web mapping applications. This framework was used immensely in the creation of the map, the placing of search and neighborhood tags  on the map and updating based UI controls.', imageIndex: 1, id: 'img-1' },
                { imageURL: '../../images/nominatim.png', imageALT: 'nominatim Logo', imageDescription: 'Nominatim, otherwise referred to as OpenStreetMap (OSM) is a collaborative project to create a free editable database of the world. This database was queried through the nominatim API to retrieve data about the Minnesota area. The marker popups displays this data.', imageIndex: 2, id: 'img-2' },
                { imageURL: '../../images/vue.png', imageALT: 'Vue Logo', imageDescription: 'Derived from AngularJS, Vue is a performant and versatile framework for building web user interfaces. It builds on top of standard  HTML, CSS, and JavaScript to make development easier and less cumbersome.  It was used in the main organization of this project.', imageIndex: 3, id: 'img-3' },
                { imageURL: '../../images/magicpattern.png', imageALT: 'MagicPattern Image', imageDescription: 'Magic Pattern Color Pallete is a website that has 10+ tools that are great for front end developers when designing and implementing the client-side view. When developing this webpage, the CSS Gradient Generator was especially useful when designing the home page. ', imageIndex: 4, id: 'img-4' },
                { imageURL: '../../images/colorkit-logo.png', imageALT: 'ColorKit Logo', imageDescription: 'ColorKit - Where Color comes to thrive... This webpage has tons of free developer tools when designing your next webpage. I used to color palette generator when designing the colors to be used on this site. ', imageIndex: 5, id: 'img-5' },
                { imageURL: '../../images/nationalweatherservice.png', imageALT: 'National Weather Service Logo', imageDescription: 'National Weather Serivce API', imageIndex: 6, id: 'img-6' },
                { imageURL: '../../images/10015.png', imageALT: '10015.io', imageDescription: 'This is a free online tool for generating CSS loaders. This site contains hundreds of loaders and spinners. This aspect of any website is needed as it provides a better user experience as well as make the webpage look more professional. ', imageIndex: 7, id: 'img-7' },
                { imageURL: '../../images/placeholder.png', imageALT: 'Placeholder Image', imageDescription: '', imageIndex: 8, id: 'img-8' },
                { imageURL: '../../images/placeholder.png', imageALT: 'Placeholder Image', imageDescription: '', imageIndex: 9, id: 'img-9' },
                { imageURL: '../../images/placeholder.png', imageALT: 'Placeholder Image', imageDescription: '', imageIndex: 10, id: 'img-10' },
                { imageURL: '../../images/placeholder.png', imageALT: 'Placeholder Image', imageDescription: '', imageIndex: 11, id: 'img-11' }
            ], 
            hasLoaded: false
        }
    },
    computed: {
        computeYear() {
            return this.footerYear; 
        }, 
        computePage() {
            return this.currentPage; 
        }
        
    },
    watch: {
        computeYear(condition) {
            $("#current-year").html(`${condition}`)
        }, 
        computePage(page) {
            if (page === "about" && this.hasLoaded == false) {
                $(".about_img").each(function(i, obj) {
                    $(obj).attr("src", $(obj).attr("data"));
                });
                this.hasLoaded = true;
            }
        }
    },
    components: {
        Footer
    },
    mounted() {
        $('.card').click(function () {
            let $this = $(this);
            let $face_front = $this.children('.face_front');
            let $face_back = $this.children('.face_back');
            let $back_btn = $this.children('.back-btn');
            if (!$this.hasClass("flipped")) {
                $(".face_front").addClass('off');
                $this.children(".face").removeClass('off');
                $this.parent(".cards").addClass('big');
                $this.addClass('flipped');
                $face_front.removeClass("visible");
                $face_front.addClass("invisible");
                $face_back.addClass("visible");
                $face_back.removeClass("invisible");
                $back_btn.css('opacity', '1');
                $back_btn.css('transition', '0.5s');
                $back_btn.data('clicked', false);
            } else if ($back_btn.data('clicked')) {
                $(".face_front").removeClass('off');
                $(".cards").removeClass('big');
                $(this).removeClass('flipped');
                $(this).children('.face_front').addClass("visible");
                $(this).children('.face_front').removeClass("invisible");
                $(this).children('.face_back').removeClass("visible");
                $(this).children('.face_back').addClass("invisible");
            }
            $back_btn.click(function () {
                $(this).css("opacity", '0');
                $(this).data('clicked', true);
            })
        });
        $('.image-content').on({
            mouseenter: function() {
                 $("#current-image").css('opacity', '0.2');
                $(".image-content").css('opacity', '1');
            }, 
            mouseleave: function() {
                $('#current-image').css('opacity', '1');
                $(".image-content").css('opacity', '0');
            }           
        });
    },
    methods: {
        swapImage(path, content, index, alt) {
            // get current displayed content
            let $currImg = $('#current-image').attr('src');
            let $currImgContent = $('.image-content').html();
            let $currImgAlt = $('#current-image').attr('alt');
            // update large image display
            $('.image-content').html(content);
            $('#current-image').attr('src', `${path}`);
            $('#current-image').attr('alt', `${alt}`)
            // update clicked thumbnail
            $(`#img-${index}`).attr('src', `${$currImg}`);
            // Update Array
            this.imageData[index].imageURL = $currImg;
            this.imageData[index].imageDescription = $currImgContent;
            this.imageData[index].imageALT = $currImgAlt;
        }
    }
}

</script>
<template>
    <div class="main_container">
        <div class="box_container">
            <div class="grid-x">
                <div class="large-2 medium-1 small-0 cell"></div>
                <div class="large-8 medium-10 small-12 cell">
                    <div style="height: 100px;"></div>
                    <div class="body_container">
                        <div id=container>

                            <div class=cards style="top:0px">
                                <div class=card>
                                    <div class="face_front visible">
                                        <h1>Mission</h1>
                                    </div>
                                    <img src="" data="../../images/x.png" class="back-btn about_img" alt="Back Button">
                                    <div class="face_back invisible">
                                        <h1>Mission</h1>
                                        <p class="about_para">Our mission is to provide a comprehensive and
                                            user-friendly platform for
                                            Minnesota travel and camping enthusiasts. We are dedicated to collecting,
                                            organizing and sharing the best information and solutions for camping trips,
                                            so that our users can easily plan and enjoy their outdoor adventures. With
                                            an emphasis on a intuitive interface and streamlined data organization, we
                                            strive to make finding the perfect camping spot as effortless as possible,
                                            while fostering a community of like-minded individuals who love to explore
                                            Minnesota's natural beauty.
                                        </p>
                                    </div>
                                </div>
                            </div>

                            <!-- <div class=cards style="top:25%">
                                <div class=card>
                                    <div class="face_front visible">
                                        <h1>Team</h1>
                                    </div>
                                    <img src="" data="../../images/x.png" class="back-btn about_img" alt="Back Button">
                                    <div class="face_back invisible">
                                        <h1>Team</h1>
                                        <div class="profile-img-div">
                                            <img id="profile_img" class="about_img" src="" data="../../images/Tucker Johnson.png" alt="Tucker Johnson Profile Image">
                                        </div>
                                        <p class="about_para">Hi, I'm Tucker Johnson and I'm the main developer behind
                                            this website. My love for the great outdoors, hiking, camping, and
                                            photography inspired me to create this platform. I wanted to provide an easy
                                            and powerful way to visualize campsites in relation to places throughout Minnesota
                                            like state parks and state forests that allow us to reconnect to nature. With my 
                                            experience in web development and programming in general, I'm excited to share
                                            this user-friendly interface that makes finding the perfect camping spot a breeze. 
                                            When I'm not working on this website, you would most likely find me enjoying a state park,
                                            taking pictures of an inspiring and amazing landscape or admiring the Milky Way. Come join me on my 
                                            mission to discover the best camping spots in Minnesota and share them with others 
                                            along the way. 
                                        </p>
                                    </div>
                                </div>
                            </div> -->

                            <div class=cards style="top:25%">
                                <div class=card>
                                    <div class="face_front visible">
                                        <h1>Technology</h1>
                                    </div>
                                    <img src="" data="../../images/x.png" class="back-btn about_img" alt="Back Button">
                                    <div class="face_back invisible">
                                        <h1>Technology</h1>
                                        <div class="image_gallery">
                                            <!-- image thumbnails clicked to bring image into full view location -->
                                            <div id="img-div">
                                                <img src="" data="../../images/foundation.svg"
                                                    alt="Image 1" id="current-image" class="about_img">
                                                <p class="image-content">Foundation is a free and open-source 
                                                    responsive front-end framework, providing a responsive grid 
                                                    and HTML and CSS UI. This framework  was used in this project 
                                                    for the general responsive formatting of the page as well as 
                                                    providing a nice looking UI.
                                                </p>
                                            </div>
                                            <div class="buffer show-for-small-only"></div>
                                            <div class="thumbnails">
                                                <img class="thumb-img about_img" :id="element.id"
                                                    @click="swapImage(element.imageURL, element.imageDescription, element.imageIndex, element.imageALT)"
                                                    v-for="(element, index) in imageData" :data="element.imageURL"
                                                    :alt="element.imageALT">
                                            </div>
                                            <div class="info_sub">
                                                Click (or tap if on a mobile device) on a thumbnail to see the image enlarged. Hover (or tap if on a mobile device) over the enlarged image to see the description. 
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- <div class=cards style="top:75%">
                                <div class=card>
                                    <div class="face_front visible">
                                        <h1>Coming Soon...</h1>
                                    </div>
                                    <img src="" data="../../images/x.png" class="back-btn about_img" alt="Back Button">
                                    <div class="face_back invisible">
                                        <h1>Coming Soon...</h1>
                                    </div>
                                </div>
                            </div> -->

                        </div>
                    </div>
                    <div id="team_space_div" class="show-for-small-only"></div>
                </div>
                <div class="large-2 medium-1 small-0 cell"></div>

            </div>
        </div>
    </div>

    <Footer :year="this.appYear"></Footer>

</template>
<style>
.main_container {
    height: fit-content;
    /* width: 100vw; */
    margin: 0px;
    margin-left: -5px; 
    margin-top: -10px; 
    background-size: cover;
    background-image: url("../../images/wall-min.jpg");
    background-repeat: no-repeat;
}

#team_space_div { 
    height: 700px; 
}

.body_container {
    display: block; 
    /* overflow: auto;  */
    /* height: 100vh; */
    width: 100%;
    margin: 0px;
}

#container {
    position: relative;
    height: 1000px;
    width: 80%;
    margin: auto;
}

.off {
    color: rgba(0, 0, 0, 0.0) !important;
    background: rgba(230, 230, 250, 0.0) !important;
    -webkit-transition: all 2s;
    /* Safari */
    transition: all 2s;
}

.card {
    background-color: rgba(255, 255, 255, 0);
    border-color: rgba(255, 255, 255, 0);
}

.cards {
    -webkit-perspective: 900px;
    -moz-perspective: 900px;
    perspective: 900px;
    width: 100%;
    height: 20%;
    position: absolute;
    -webkit-transition: all 1s;
    /* Safari */
    transition: all 1s;
    margin-bottom: 100px;
}

.cards .card.flipped {
    -webkit-transform: rotateY(-180deg);
    -moz-transform: rotateY(-180deg);
    transform: rotateY(-180deg);
    height: 100%;
    z-index: 100;
}

.cards .card {
    width: 100%;
    height: 100%;
    -webkit-transform-style: preserve-3d;
    -moz-transform-style: preserve-3d;
    transform-style: preserve-3d;
    -webkit-transition: all 1s;
    /* Safari */
    transition: all 1s;
    cursor: pointer;
    border-radius: 20px;
}

.face_front {
    position: absolute;
    background-color: rgba(255, 255, 255, 0.8);
    height: 100%;
    width: 100%;
    backface-visibility: hidden;
    transition: 0.5s;
    display: flex;
    justify-content: center;
    align-items: center;
}

.face_front h1 {
    font-size: 3rem;
    /* text-align: center;  */
}

.face_back {
    transform: rotateY(180deg);
    background-color: rgba(255, 255, 255, 0.8);
    /* backface-visibility: hidden; */
    transition: 0.5s;
    height: 100%;
    width: 100%;
    z-index: 1;
}

.face_back h1 {
    padding: 20px;
    font-size: 3rem;
    text-align: center;
}

.visible {
    opacity: 1;
}

.invisible {
    opacity: 0;
}

.big {
    height: fit-content;
    width: 100%;
    top: 0% !important;
    margin-left: 0%;
    margin-right: 0%;
    z-index: 100;
}

.back-btn {
    opacity: 0;
    position: fixed;
    z-index: 2;
    left: 10px;
    top: 10px;
    transition: 0.5s;
}

.back-btn:hover {
    filter: invert(1);
}

.about_para {
    padding: 20px;
    font-size: 1.1rem;
}

.profile-img-div {
    text-align: center; 
    width: 100%; 
    height: 200px; 
}

#profile_img {
    /* height: 10%;  */
    height: 100%; 
}

.image_gallery {
    text-align: center;
    margin: 0 auto;
}

#current-image {
    width: 50%;
    height: 20%;
    transition: 0.5s;
}

#img-div {
    width: 100%;
    height: 100%;
    text-align: center;
}

.image-content {
    position: fixed;
    top: 0%;
    left: 15%;
    right: 15%;
    margin-top: 150px;  
    opacity: 0;
    transition: 0.5s;
    height: 35%; 
    max-height: 150px; 
    overflow-y: scroll;
}

#current-image:hover {
    opacity: 0.2;
    transition: 0.5s;
}

#current-image:hover+.image-content {
    opacity: 1;
    transition: 0.5s;
}

.thumbnails {
    display: flex;
    justify-content: center;
    margin-top: 20px;
    flex-wrap: wrap;
}

.thumb-img {
    flex: 1 0 11%;
    margin: 10px 10px;
    height: 5%;
    width: 5%;
}

.small_buffer {
    height: 200px; 
}
</style>