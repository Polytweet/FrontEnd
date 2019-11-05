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
      <div v-if="communes.length>0 && currentSideState">
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
          <template slot-scope="props" v-if="communes.length>0">
            <l-info-control
              :item="props.currentItem"
              :unit="props.unit"
              title="Département"
              placeholder="Survolez un département"
              position="topleft"
            ></l-info-control>
          </template>
        </l-choropleth-layer>
      </div>
    </l-map>
  </div>
</template>

 
<script>
import { InfoControl, ChoroplethLayer } from "vue-choropleth";

import { LMap, LTileLayer } from "vue2-leaflet";
import axios from "axios";
import gql from "graphql-tag";

export default {
  name: "CarteCompo",
  components: {
    LMap,
    LTileLayer,
    "l-info-control": InfoControl,
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
      url:
        "https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png",
      zoom: 12,
      center: [44.762632, 5.348876],
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
    this.getCurrentLocation();
    this.getDeptDataFromGeoJson();
  },
  created() {
    window.addEventListener('click', () => {
      switch (this.$store.state.currentFilter) {
        case "region": 
          this.zoomUpdated(6);
          break;
        case "departement":
          this.zoomUpdated(8);
          this.value.key = 'rien';
          this.value.metric = '';
          this.extraValues = [];
          break;
        case "communes":
          this.zoomUpdated(12);
          break;
        default:
          this.zoom(13);
      }
    })
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
      axios.get('https://api.opencagedata.com/geocode/v1/json?q=' + event.latlng.lat + '+' + event.latlng.lng + '&key=6cb782be82c646cfb05d9471b7ca2961').then((res) => {
        console.log(res.data.results[0].components.postcode.substring(0,2))
        this.$store.state.currentFilter = 'commune'
        this.zoomUpdated(12);
      })
    },

    getCurrentLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          position => {
            //do work work here
            console.log(position);

            this.center = [position.coords.latitude, position.coords.longitude];
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

    async getDataFromGeoJson() {
      let resApollo = await this.$apollo.query({
        query: gql`
          query {
            communes(code_dept: "42") {
              type
              properties: fields {
                population
                superficie
                code_com
                nom_com
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
      resApollo.data.communes.forEach(element => {
        this.communesInfo.push(element.properties);
      });
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
      this.communes.forEach((c) => {
        if (c.geometry.coordinates.length === 0) {
          c.geometry.coordinates = c.geometry.coordinatesMulti
        }
      })
      resApollo.data.departements.forEach(element => {
        element.properties.rien = ''
        this.communesInfo.push(element.properties);
      });
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
</style>
