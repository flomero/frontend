<template>
  <div class="home">
    <l-map class="map" 
           ref="map" 
           :center="center" 
           :zoom=7 
           :options="mapOptions">
      <l-tile-layer :url="url" 
                    :attribution="attribution"/>
      <l-marker @click="clickMarker(demo)" 
                v-for="demo in demos" 
                :icon="demo.icon" 
                :lat-lng="demo.latLng" 
                :key="demo.id"/>
    </l-map>
    <fffMarkerDetails :model="selectedMarker" 
                      @interface="handleCloseMarkerDetails"/>
  </div>
</template>

<script>

import L from 'leaflet';
import { LMap, LTileLayer, LMarker } from 'vue2-leaflet';

import fffMarkerDetails from '../components/markerDetails.vue';

export default {
  name: 'home',
  data() {
    return {
      mapOptions: {},
      url: 'https://{s}.tile.osm.org/{z}/{x}/{y}.png',
      attribution: 'Map data © <a href="https://openstreetmap.org">OpenStreetMap</a> contributors',
      center: new L.LatLng(50.9, 10.6),
      selectedMarker: null
    };
  },
  computed: {

    demos() {

      return this.$store.getters['demos/getItems'];
    }
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    fffMarkerDetails
  },
  methods: {
    clickMarker(demo) {

      console.log(demo);
      this.selectedMarker = demo;
    },
    handleCloseMarkerDetails() {

      this.selectedMarker = null;
    } 
  },
  beforeCreate() {

    this.$store.dispatch('demos/getList');
    this.$store.dispatch('localgroups/getList');
  }
};
</script>

<style lang="scss">

  .home {
    height: 100%;
    width: 100%;
  }

</style>
