<script>
import Header from './components/header.vue'
import Map from './components/map.vue'
import About from './components/about.vue'
import Home from './components/home.vue'
import Form from './components/form.vue'
export default {
  data() {
    return {
      view: "home",
      formcondition: false,
      buildMapCondition: false,
      currYear: new Date().getFullYear()
    }
  },
  components: {
    Header,
    Map,
    About,
    Home,
    Form
  },
  computed: {
    viewCondition() {
      return this.view;
    },
  },
  watch: {
    viewCondition(value) {
      if (value == "map") {
        this.buildMapCondition = true;
      }
    }
  },
  mounted() {
    const $button = $('#header_btn');
    $button.css("display", "none");
  },
  methods: {
    updatePage(page) {
      this.view = page;
      if (page == 'map') {
        const $button = $('#header_btn');
        $button.css("display", "");
      }

    }
  }
}
</script>

<template>
  <div class="off-canvas-content">
    <Header @currPage="updatePage($event)"></Header>
    <div v-show="view === 'home'">
      <Home :appYear="this.currYear" @executeMap="updatePage($event)"></Home>
    </div>
    <div v-show="view === 'map'">
      <div class="grid-x">
        <div class="large-12 medium-12 small-12 cell">
          <Map :buildmap="this.buildMapCondition"></Map>
        </div>
      </div>
    </div>
    <div v-show="view === 'about'">
      <About :appYear="this.currYear" :currentPage="this.view"></About>
    </div>
    <div v-show="view === 'form'">
      <Form :appYear="this.currYear" :currentPage="this.view"></Form>
    </div>
  </div>
</template>

<style>
#map {
  position: absolute !important;
}
</style>
