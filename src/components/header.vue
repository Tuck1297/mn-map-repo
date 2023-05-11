<script>

export default {
  emits: ["currPage"],
  data() {
    return {
      className: "headerAnimation_down", 
      currPageVal: 'home'
    }
  },
  mounted() {
    $('#small_nav_btn').click(() => {
      $('.js-off-canvas-overlay').addClass('is-visible');
      $('.js-off-canvas-overlay').addClass('is-closable');
      $('#offCanvasNestedOver').addClass('is-open');
      $('#sidebar').css('display', 'none');
      // $('#header_btn').css('display', 'none');
    });

    $('.js-off-canvas-overlay').click(() => {
      $('.js-off-canvas-overlay').removeClass('is-visible');
      $('.js-off-canvas-overlay').removeClass('is-closable');
      $('#offCanvasNestedOver').removeClass('is-open');
      $('#sidebar').css('display', '');
      // $('#header_btn').css('display', '');
    });

    $('#nav_close').click(() => {
      $('.js-off-canvas-overlay').removeClass('is-visible');
      $('.js-off-canvas-overlay').removeClass('is-closable');
      $('#offCanvasNestedOver').removeClass('is-open');
      $('#sidebar').css('display', '');
      // $('#header_btn').css('display', '');
    })
  },
  methods: {
    passEvent(page) {
      const $button = $('#header_btn');
      if (page === 'home' || page === 'about' || page === 'form') {
        const $header = $('#header');
        const $header_sm = $('#header_small')
        $header_sm.addClass('headerAnimation_down').removeClass('headerAnimation_up');
        $header.addClass('headerAnimation_down').removeClass('headerAnimation_up');
        $button.css('display', 'none');
      } else {
        $button.css('display', '');
      }

      this.$emit('currPage', page)
    },
    toggleHeader() {
      const $header = $('#header');
      const $header_sm = $('#header_small')
      const $button = $('#header_btn');
      const $search = $('#search');

      const currentClass = $header.attr('class');
      const hasDownClass = currentClass.includes('headerAnimation_down');

      if (hasDownClass) {
        $header.addClass('headerAnimation_up').removeClass('headerAnimation_down');
        $header_sm.addClass('headerAnimation_up').removeClass('headerAnimation_down');
        $button.addClass('point_up').removeClass('point_down');
        $search.addClass('animation_up').removeClass('animation_down');

      } else {
        $header.addClass('headerAnimation_down').removeClass('headerAnimation_up');
        $header_sm.addClass('headerAnimation_down').removeClass('headerAnimation_up');
        $button.addClass('point_down').removeClass('point_up');
        $search.addClass('animation_down').removeClass('animation_up');
      }
    },
  }
}

</script>

<template>
  <div class="grid-x">
    <div class="large-12 medium-12 small-12 cell">
      <div class="headerAnimation_down show-for-small-only" id="header_small">
        <button class="justify-left menu-icon" type="button" id="small_nav_btn"></button>
        <img class="icon_image justify-right" src="../../images/map_minnesota_icon-min.png" alt="Map MN Logo">
          <div class="js-off-canvas-overlay is-overlay-fixed"></div>
          <div class="off-canvas position-left" id="offCanvasNestedOver">
            <button id="nav_close" class="close_position" type="button"><img src="../../images/x.png" alt="Close Button"></button>
            <div class="nav_links">
              <div class="buffer"></div>
              <button class="side_nav_btn" @click="passEvent('home')">HOME</button>
              <button class="side_nav_btn" @click="passEvent('map')">MAP</button>
              <button class="side_nav_btn" @click="passEvent('about')">ABOUT</button>
              <button class="side_nav_btn" @click="passEvent('form')">FORM</button>
            </div>
            
          </div>
      </div>
      <div class="headerAnimation_down show-for-medium" id="header">
        <div id="nav">
          <button class="nav_btn" @click="passEvent('home')">HOME</button>
          <button class="nav_btn" @click="passEvent('map')">MAP</button>
          <img class="icon_image" src="../../images/map_minnesota_icon-min.png" alt="Map MN Logo">
          <button class="nav_btn" @click="passEvent('about')">ABOUT</button>
          <button class="nav_btn" @click="passEvent('form')">FORM</button>
        </div>

      </div>
    </div>
  </div>
  <button @click="toggleHeader" id="header_btn" class=" header_btn point_down"><img src="/images/up-arrow.png"
      alt="Up Arrow"></button>
</template>

<style>
.justify-left {
  position: absolute;
  left: 25px;
  top: 45px;
}

.justify-right {
  position: absolute;
  right: 70px;
}

.close_position {
  position: fixed; 
  top: 10px; 
  right: 10px; 
}

#nav_close:hover {
    filter: brightness(0) invert(1);
    cursor: pointer;
}

.nav_links {
  display: flex; 
  flex-direction: column;
  justify-content: center;
}

.side_nav_btn {
   padding: 20px; 
   height: 70px; 
   font-size: 1.75rem; 
   text-decoration: underline; 
   color: black; 
}

.side_nav_btn:hover {
  background-color: var(--main_color_2);
  cursor: pointer;
}

#nav {
  display: flex;
  justify-content: center;
}

.nav_btn {
  margin-top: 0px;
  padding-left: 10px;
  padding-right: 10px;
  height: 70px;
  font-size: 1.2rem;
  text-decoration: underline;
  color: white;
  cursor: pointer; 
}

.icon_image {
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 10px;
  padding-right: 10px;
  height: 70px;
}

.nav_btn {
  color: white;
}

.nav_btn:hover {
  background-color: var(--main_color_2);
}

#header {
  position: absolute;
  width: 100%;
  height: 90px;
  padding: 20px;
  z-index: 3;
  /* background-color: var(--main_color_1); */
  background-color: rgba(67, 82, 165, 0.7);
}

#header_small {
  position: absolute;
  width: 100%;
  height: 90px;
  padding: 20px;
  z-index: 3;
  /* background-color: var(--main_color_1); */
  background-color: rgba(67, 82, 165, 0.7);
}

.headerAnimation_up {
  top: -90px;
  animation: header_up_animation 1s forwards normal 1;
  -webkit-animation: header_up_animation 1s forwards normal 1;
  -moz-animation: header_up_animation 1s forwards normal 1;
  -ms-animation: header_up_animation 1s forwards normal 1;
  -o-animation: header_up_animation 1s forwards normal 1;
}

.headerAnimation_down {
  top: -20px;
  animation: header_down_animation 1s forwards normal 1;
  -webkit-animation: header_down_animation 1s forwards normal 1;
  -moz-animation: header_down_animation 1s forwards normal 1;
  -ms-animation: header_down_animation 1s forwards normal 1;
  -o-animation: header_down_animation 1s forwards normal 1;
}

.header_btn {
  position: absolute;
  z-index: 3;
  background-color: var(--main_color_2);
  top: 10px;
  right: 20px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 4px double black;
  cursor: pointer; 
}

.point_up {
  transform: rotateZ(180deg);
  transition: transform 0.5s;
}

.point_down {
  transform: rotateZ(0deg);
  transition: transform 0.5s;
}

.point_down:hover {
  background-color: var(--main_color_4);
  transform: scale(1.2) rotateZ(0deg);
}

.point_up:hover {
  background-color: var(--main_color_4);
  transform: scale(1.2) rotateZ(180deg);
}

.animation_up {
  top: 10px;
  animation: search_up_animation 1s forwards normal 1;
  -webkit-animation: search_up_animation 1s forwards normal 1;
  -moz-animation: search_up_animation 1s forwards normal 1;
  -ms-animation: search_up_animation 1s forwards normal 1;
  -o-animation: search_up_animation 1s forwards normal 1;
}

.animation_down {
  top: 80px;
  animation: search_down_animation 1s forwards normal 1;
  -webkit-animation: search_down_animation 1s forwards normal 1;
  -moz-animation: search_down_animation 1s forwards normal 1;
  -ms-animation: search_down_animation 1s forwards normal 1;
  -o-animation: search_down_animation 1s forwards normal 1;
}

@keyframes header_up_animation {
  0% {
    top: -20px;
    left: 0px;
  }

  10% {
    top: 0px;
    left: 0px;
  }

  100% {
    top: -90px;
    left: 0px;
  }

}

@keyframes header_down_animation {
  0% {
    top: -90px;
    left: 0px;
  }

  50% {
    top: 0px;
    left: 0px;
  }

  100% {
    top: -20px;
  }
}

@keyframes search_up_animation {
  0% {
    top: 80px;
  }

  10% {
    top: 100px;
  }

  100% {
    top: 10px;
  }

}

@keyframes search_down_animation {
  0% {
    top: 10px;
  }

  50% {
    top: 100px;
  }

  100% {
    top: 80px;
  }
}
</style>