<template>
  <div>
    <l-map
      :zoom="zoom"
      :center="center"
      @update:zoom="zoomUpdated"
      @update:center="centerUpdated"
      @update:bounds="boundsUpdated"
      @click="(event)=>getCoord(event)"
      :options="{zoomControl: false}"
    >
      <l-tile-layer :url="url"></l-tile-layer>
      <div v-if="(communes ? communes.length>0 : false) && currentSideState">
        <l-choropleth-layer
          :data="communesInfo"
          titleKey="nom"
          idKey="code"
          :value="value"
          :extraValues="extraValues"
          geojsonIdKey="code"
          :geojson="{'type':'FeatureCollection','totalFeatures':18,'features':communes}"
          :colorScale="colorScale"
        >
          <template slot-scope="props" v-if="(communes ? communes.length>0 : false)">
            <div
              class="card"
              v-bind:class="{ 'cardExpand': cardIsExpand }"
              @mouseup="cardIsExpand=true"
              @mousedown="cardIsExpand=false"
            >
              <div class="card-body">
                <h5 class="card-title">Département</h5>
                <h6 class="card-subtitle mb-2 text-muted">{{props.currentItem.name}}</h6>
                <div
                  v-if="props.currentItem.value!=undefined && props.currentItem.extraValues!=undefined && props.currentItem.extraValues[0]!=undefined"
                >
                  <span>{{props.currentItem.value}}hab</span>
                  <br />
                  <span>{{props.currentItem.extraValues[0].value}}{{props.currentItem.extraValues[0].metric}}</span>
                </div>
              </div>
            </div>

            <!--   <l-info-control
              :item="props.currentItem"
              :unit="props.unit"
              title="Département"
              placeholder="Survolez un département"
              position="topleft"
            ></l-info-control>-->
          </template>
        </l-choropleth-layer>
      </div>
    </l-map>
  </div>
</template>

 
<script>
import { ChoroplethLayer } from "vue-choropleth";

import { LMap, LTileLayer } from "vue2-leaflet";
import axios from "axios";
import gql from "graphql-tag";

export default {
  name: "CarteCompo",
  components: {
    LMap,
    LTileLayer,
    "l-choropleth-layer": ChoroplethLayer
  },
  props: {
    currentSideState: {
      type: Boolean
    }
  },
  watch: {
    currentSideState: function() {}
  },
  data() {
    return {
      cardIsExpand: false,
      url:
        "https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png",
      zoom: 6,
      center: [46.443004, 2.878054],
      bounds: null,
      communes: {},
      communesInfo: [],
      pyDepartmentsData: [],
      colorScale: ["e7d090", "e9ae7b", "de7062"],
      value: {
        key: "population",
        metric: " hab"
      },
      extraValues: [
        {
          key: "superficie",
          metric: " ha"
        }
      ],
      mapOptions: {
        attributionControl: false
      },
      currentStrokeColor: "333333"
    };
  },
  apollo: {
    // Simple query that will update the 'communes' vue property
    // communes: gql`
    //   query {
    //     communes(code_dept: "42") {
    //       type
    //       properties: fields {
    //         population
    //         superficie
    //         code_com
    //         nom_com
    //       }
    //       geometry {
    //         type: _type
    //         coordinates
    //       }
    //     }
    //   }
    // `
  },
  mounted() {
    this.getDeptDataFromGeoJson();
    // this.getDataFromGeoJson(69)
  },
  created() {
    window.addEventListener("click", () => {
      switch (this.$store.state.currentFilter) {
        case "franceN":
          this.zoomUpdated(6);
          this.getDeptDataFromGeoJson();
          this.value.key = "rien";
          this.value.metric = "";
          this.extraValues = [];
          this.$store.state.currentFilter = "france";
          break;
        case "departementN":
          this.zoomUpdated(7);
          this.getDeptDataFromGeoJson();
          this.value.key = "rien";
          this.value.metric = "";
          this.extraValues = [];
          this.$store.state.currentFilter = "departement";
          break;
        case "communeN":
          this.zoomUpdated(10);
          this.value.key = "population";
          this.value.metric = "hab";
          this.extraValues = [
            {
              key: "superficie",
              metric: " ha"
            }
          ];
          this.$store.state.currentFilter = "commune";
          break;
      }
    });
    window.addEventListener("load", () => {
      document
        .getElementsByClassName("getPosition")[0]
        .addEventListener("click", () => {
          this.getCurrentLocation();
        });
    });
  },
  methods: {
    zoomUpdated(zoom) {
      this.zoom = zoom;
    },
    centerUpdated(center) {
      this.center = center;
    },
    boundsUpdated(bounds) {
      this.bounds = bounds;
    },
    getCoord(event) {
      if (this.$store.state.currentFilter === "departement") {
        axios
          .get(
            "https://api.opencagedata.com/geocode/v1/json?q=" +
              event.latlng.lat +
              "+" +
              event.latlng.lng +
              "&key=6cb782be82c646cfb05d9471b7ca2961"
          )
          .then(res => {
            console.log(
              res.data.results[0].components.postcode.substring(0, 2)
            );
            (this.center = [event.latlng.lat, event.latlng.lng]),
              (this.$store.state.currentFilter = "communeN");

            this.getDataFromGeoJson(
              res.data.results[0].components.postcode.substring(0, 2)
            );
          });
      }
      if (this.$store.state.currentFilter === "france") {
        (this.center = [event.latlng.lat, event.latlng.lng]),
          (this.$store.state.currentFilter = "departementN");
      }
    },

    getCurrentLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          position => {
            //do work work here
            console.log(position);

            this.center = [position.coords.latitude, position.coords.longitude];
            axios
              .get(
                "https://api.opencagedata.com/geocode/v1/json?q=" +
                  position.coords.latitude +
                  "+" +
                  position.coords.longitude +
                  "&key=6cb782be82c646cfb05d9471b7ca2961"
              )
              .then(res => {
                console.log(
                  res.data.results[0].components.postcode.substring(0, 2)
                );
                this.$store.state.currentFilter = "communeN";

                this.getDataFromGeoJson(
                  res.data.results[0].components.postcode.substring(0, 2)
                );
              });
          },
          function(error) {
            alert(error.message);
          },
          {
            enableHighAccuracy: true,
            timeout: 500000
          }
        );
      } else {
        alert("Geolocation is not supported by this browser.");
      }
      return null;
    },

    async getDataFromGeoJson(codeDept) {
      let resApollo = await this.$apollo.query({
        query: gql`
          query {
            communes(code_dept: "${codeDept}") {
              type
              properties: fields {
                population
                superficie
                code: code_com
                nom: nom_com
              }
              geometry {
                type: _type
                coordinates
                coordinatesMulti
              }
            }
          }
        `
      });
      this.communes = resApollo.data.communes;
      this.communesInfo = [];
      resApollo.data.communes.forEach(element => {
        element.properties.rien = "";
        this.communesInfo.push(element.properties);
      });
      console.log(this.communes);
      this.updateZoom();
    },
    updateZoom() {
      switch (this.$store.state.currentFilter) {
        case "franceN":
          this.zoomUpdated(6);
          this.getDeptDataFromGeoJson();
          this.value.key = "rien";
          this.value.metric = "";
          this.extraValues = [];
          this.$store.state.currentFilter = "france";
          break;
        case "departementN":
          this.zoomUpdated(7);
          this.getDeptDataFromGeoJson();
          this.value.key = "rien";
          this.value.metric = "";
          this.extraValues = [];
          this.$store.state.currentFilter = "departement";
          break;
        case "communeN":
          this.zoomUpdated(10);
          this.value.key = "population";
          this.value.metric = "hab";
          this.extraValues = [
            {
              key: "superficie",
              metric: " ha"
            }
          ];
          this.$store.state.currentFilter = "commune";
          break;
      }
    },
    async getDeptDataFromGeoJson() {
      let resApollo = await this.$apollo.query({
        query: gql`
          query {
            departements {
              type
              properties {
                code
                nom
              }
              geometry {
                type: _type
                coordinates
                coordinatesMulti
              }
            }
          }
        `
      });
      this.communes = resApollo.data.departements;
      this.communes.forEach(c => {
        if (
          c.geometry.coordinates ? c.geometry.coordinates.length === 0 : false
        ) {
          c.geometry.coordinates = c.geometry.coordinatesMulti;
        }
      });
      this.communesInfo = [];
      resApollo.data.departements.forEach(element => {
        element.properties.rien = "";
        this.communesInfo.push(element.properties);
      });
      this.updateZoom();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.map,
.vue2leaflet-map {
  height: 100vh;
  width: 100vw;
}

.card {
  z-index: 1000;
  width: 20%;
  position: absolute;
  left: 3%;
  top: 3%;
  min-height: 66px;
  box-shadow: 0 1.25rem 1.562rem -1.25rem rgba(0, 0, 0, 0.2);
  border: 1px solid #eaf1f3;
  background-color: #fff !important;
}
.card-body {
  padding: 1rem;
}

@media screen and (max-width: 540px) {
  .card {
    position: absolute;
    top: auto;
    left: 0;
    bottom: 0;
    width: 100vw;
    min-height: 75px;
    max-height: 75px;
  }
  .cardExpand {
    height: 50vh;
    min-height: 50vh;
    max-height: 50vh;
  }
}
</style>
