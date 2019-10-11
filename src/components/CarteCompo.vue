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
      <div v-if="communes.length>0 ">
        <l-choropleth-layer
          :data="communesInfo"
          titleKey="nom_com"
          idKey="code_com"
          :value="value"
          :extraValues="extraValues"
          geojsonIdKey="code_com"
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
import gql from "graphql-tag";

export default {
  name: "CarteCompo",
  components: {
    LMap,
    LTileLayer,
    "l-info-control": InfoControl,
    "l-choropleth-layer": ChoroplethLayer
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
        metric: " ha"
      },
      extraValues: [
        {
          key: "superficie",
          metric: " km<sup>2</sup>"
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
    communes: gql`
      query {
        communes(code_dept: "69") {
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
          }
        }
      }
    `
  },
  mounted() {
    this.getCurrentLocation();
    this.getDataFromGeoJson();
  },
  created() {},
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
      console.log(event);
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
            communes(code_dept: "69") {
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
              }
            }
          }
        `
      });
      resApollo.data.communes.forEach(element => {
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
