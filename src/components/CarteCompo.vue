<template>
  <div class="map" id="map"></div>
</template>

  <script src="https://unpkg.com/leaflet@1.5.1/dist/leaflet.js"
        integrity="sha512-GffPMF3RvMeYyc1LWMHtK8EbPv0iNZ8/oTtHPx9/cc2ILxQ+u905qIwdpULaqDkyBKgOaB57QTMg7ztg8Jm2Og=="
        crossorigin=""></script>
 
<script>
export default {
  name: "CarteCompo",

  data() {
    return {
      latitude: 44.762632,
      longitude: 5.348876
    };
  },
  mounted() {
    this.getCurrentLocation();
  },
  created() {
    this.initLayers();
    this.septRegion();
  },
  methods: {
    getCurrentLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          position => {
            //do work work here
            console.log(position);
            this.latitude = position.coords.latitude;
            this.longitude = position.coords.longitude;
            this.initMap();
          },
          function(error) {
            alert(error.message);
            this.initMap();
          },
          {
            enableHighAccuracy: true,
            timeout: 500000
          }
        );
      } else {
        alert("Geolocation is not supported by this browser.");
        this.initMap();
      }
      return null;
    },
    initLayers() {},
    initMap() {
      this.map = L.map("map").setView([this.latitude, this.longitude], 12);
      this.tileLayer = L.tileLayer(
        "https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png",
        {
          maxZoom: 18,
          attribution:
            '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, &copy; <a href="https://carto.com/attribution">CARTO</a>'
        }
      );
      this.tileLayer.addTo(this.map);
    },
    septRegion() {
      // L.geoJson(statesData, { style: style }).addTo(map);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.map {
  height: 100vh;
  width: 100vw;
}
</style>
