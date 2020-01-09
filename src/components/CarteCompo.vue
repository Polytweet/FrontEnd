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
                <h5 class="card-title text-capitalize">{{currentFilter}}</h5>
                <h6 class="card-subtitle mb-2 text-muted">{{props.currentItem.name}}</h6>

                <div
                  v-if="props.currentItem.value!=undefined && props.currentItem.extraValues!=undefined && props.currentItem.extraValues[0]!=undefined"
                >
                  <!-- City -->
                  <!--   <span>{{props.currentItem.value}}hab</span>
                  <br />
                  <span>{{props.currentItem.extraValues[0].value}}{{props.currentItem.extraValues[0].metric}}</span>
                  <br />-->
                  <!-- TopTweet -->
                  <hr />
                  <div class="d-flex align-items-center justify-content-center">
                    <h6 class="card-subtitle my-0 mr-1 text-muted">TopTweet</h6>
                    <TwitterIcon class="fab twitter-icon text-muted" />
                  </div>

                  <div v-if="props.currentItem.extraValues[1].value.length > 0">
                    <div
                      class="list-group"
                      v-for="(hashtag, index) in props.currentItem.extraValues[1].value"
                      :key="hashtag.hashtag + hashtag.count"
                    >
                      <span v-if="index < 5" class="text-left mx-auto">#{{hashtag.hashtag}}</span>
                    </div>
                  </div>
                  <div v-else>Nous n'avons aucun tweet provenant de cet endroit.</div>

                  <!--/TopTweet -->
                </div>
              </div>
            </div>
          </template>
        </l-choropleth-layer>
      </div>
    </l-map>
  </div>
</template>

<script>
// InfoControl
import { ChoroplethLayer } from "vue-choropleth";
import { LMap, LTileLayer } from "vue2-leaflet";
import axios from "axios";
import gql from "graphql-tag";
import TwitterIcon from "@/assets/svg/twitter-brands.svg";
// "l-info-control": InfoControl,
export default {
  name: "CarteCompo",
  components: {
    LMap,
    LTileLayer,
    "l-choropleth-layer": ChoroplethLayer,
    TwitterIcon
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
      currentFilter: "Region",
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
    this.getRegDataFromGeoJson();
    // this.getDataFromGeoJson(69)
  },
  created() {
    window.addEventListener("click", () => {
      this.currentFilter = this.$store.state.currentFilter;
      switch (this.$store.state.currentFilter) {
        case "regionN":
          this.zoomUpdated(6);
          this.getRegDataFromGeoJson();
          this.value.key = "rien";
          this.value.metric = "";
          this.extraValues = [];
          this.$store.state.currentFilter = "region";
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
              res.data.results[0].components.state_district.toUpperCase()
            );
            this.center = [event.latlng.lat, event.latlng.lng];
            this.getDataFromGeoJson(
              res.data.results[0].components.state_district.toUpperCase(),
              res.data.results[0].components.postcode.substring(0, 2)
            );
          });
      }
      if (this.$store.state.currentFilter === "region") {
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
                  res.data.results[0].components.state_district.toUpperCase()
                );
                this.getDataFromGeoJson(
                  res.data.results[0].components.state_district.toUpperCase(),
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
    async getDataFromGeoJson(nomDept, codeDept) {
      var str = "";
      if (codeDept == 20) {
        str = 'nom_dept: "' + nomDept + '"';
      } else {
        str = 'code_dept: "' + codeDept + '"';
      }
      let resApollo = await this.$apollo.query({
        query: gql`
          query {
            communes(${str}) {
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
      let resApolloHash = await this.$apollo.query({
        query: gql`
          query {
            topHashtagsFromAllCitiesInOneDepartement(depCode:"${codeDept}") {
              _id
              hashtags {
                hashtag
                count
              }
            }
          }
        `
      });
      this.communes = resApollo.data.communes;
      this.communesInfo = [];
      resApollo.data.communes.forEach(element => {
        element.properties.rien = "";
        element.properties.hashtags = [];
        element.properties.hashtags1 = "";
        this.communesInfo.push(element.properties);
      });
      // console.log(resApolloHash.data.tweetsFromDepartement);
      resApolloHash.data.topHashtagsFromAllCitiesInOneDepartement.forEach(
        topHash => {
          // console.log(topHash._id)
          // console.log(topHash.hashtags)
          if (topHash._id == 13201) console.log(topHash);
          var com = this.communes.find(c => {
            if (
              c.properties.nom.toUpperCase().indexOf("LYON") == 0 &&
              topHash._id == 69123
            ) {
              return true;
            }
            if (
              c.properties.nom.toUpperCase().indexOf("MARSEILLE") == 0 &&
              topHash._id == 13201
            ) {
              return true;
            }
            return "" + codeDept + "" + c.properties.code === topHash._id;
          });
          if (com) {
            console.log(com.properties.nom);
            com.properties.hashtags = topHash.hashtags;
          }
        }
      );
      this.communes.forEach(c => {
        if (c.properties.hashtags.size !== 0) {
          c.properties.hashtags.sort((a, b) => {
            return b.count - a.count;
          });
          if (c.properties.nom.toUpperCase().indexOf("LYON") == 0) {
            this.communes.forEach(co => {
              if (co.properties.nom.toUpperCase().indexOf("LYON") == 0) {
                co.properties.hashtags = c.properties.hashtags;
              }
            });
          }
          if (c.properties.nom.toUpperCase().indexOf("MARSEILLE") == 0) {
            this.communes.forEach(co => {
              if (co.properties.nom.toUpperCase().indexOf("MARSEILLE") == 0) {
                co.properties.hashtags = c.properties.hashtags;
              }
            });
          }
          if (c.properties.hashtags.length > 0)
            c.properties.hashtags1 = c.properties.hashtags[0].hashtag;
        }
      });
      this.$store.state.currentFilter = "communeN";
      this.updateZoom();
    },
    updateZoom() {
      switch (this.$store.state.currentFilter) {
        case "regionN":
          this.zoomUpdated(6);
          this.getRegDataFromGeoJson();
          this.value.key = "rien";
          this.value.metric = "";
          this.extraValues = [];
          this.$store.state.currentFilter = "region";
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
            },
            {
              key: "hashtags"
            }
          ];
          this.$store.state.currentFilter = "commune";
          break;
      }
    },
    async getRegDataFromGeoJson() {
      let resApollo = await this.$apollo.query({
        query: gql`
          query {
            regions {
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
      this.communes = resApollo.data.regions;
      this.communes.forEach(c => {
        if (
          c.geometry.coordinates ? c.geometry.coordinates.length === 0 : false
        ) {
          c.geometry.coordinates = c.geometry.coordinatesMulti;
        }
      });
      this.communesInfo = [];
      resApollo.data.regions.forEach(element => {
        element.properties.rien = "";
        this.communesInfo.push(element.properties);
      });
      this.updateZoom();
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
.twitter-icon {
  height: 16px;
  width: 16px;
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