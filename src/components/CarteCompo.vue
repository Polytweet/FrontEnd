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
                  <!-- TopTweet -->
                  <hr />
                  <div class="d-flex align-items-center justify-content-center">
                    <h6 class="card-subtitle my-0 mr-1 text-muted">TopTweet</h6>
                    <TwitterIcon class="fab twitter-icon text-muted" />
                  </div>
                  <div
                    v-if="props.currentItem.extraValues[2].value && props.currentItem.extraValues[2].value.length > 0"
                  >
                    <div
                      class="list-group"
                      v-for="(hashtag, index) in props.currentItem.extraValues[2].value"
                      :key="hashtag.hashtag + hashtag.count"
                    >
                      <span v-if="index < 5" class="text-left mx-auto">#{{hashtag.hashtag}}</span>
                    </div>
                  </div>
                  <div v-else>Nous n'avons aucun tweet provenant de cet endroit.</div>

                  <hr />
                  <div class="d-flex align-items-center justify-content-center">
                    <h6 class="card-subtitle my-0 mr-1 text-muted">Statistiques</h6>
                    <StatIcon class="fas chart-bar-icon text-muted" />
                  </div>

                  <!-- Evolution nb Tweets per days -->
                  <div class="d-flex align-items-center justify-content-between w-100 mt-2">
                    <h6 class style="font-size:13px;">Evolution sur les dernière 24 h</h6>
                    <h6
                      class
                      style="font-size:13px;"
                      v-bind:class="[props.currentItem.extraValues[4].value>0 ? 'text-success' : 'text-danger']"
                    >
                      <span
                        v-if="props.currentItem.extraValues[4].value!==undefined"
                      >{{props.currentItem.extraValues[4].value}} %</span>
                    </h6>
                  </div>
                  <!-- /Evolution nb Tweets per days -->

                  <!-- Nb Tweets per days -->
                  <div class="d-flex align-items-center justify-content-between w-100 mt-2">
                    <h6 class style="font-size:13px;">Tweets par jours</h6>
                    <h6
                      style="font-size:13px;"
                    >{{Math.round(props.currentItem.extraValues[3].value)}}</h6>
                  </div>
                  <GaugeCompo
                    v-bind:nbTweetDay="props.currentItem.extraValues[3].value"
                    v-bind:nbTweetMax="nbTweetMax"
                  ></GaugeCompo>
                  <!-- /Nb Tweets per days -->
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
import { ChoroplethLayer } from "vue-choropleth";
import { LMap, LTileLayer } from "vue2-leaflet";
import axios from "axios";
import gql from "graphql-tag";
import TwitterIcon from "@/assets/svg/twitter-brands.svg";
import StatIcon from "@/assets/svg/chart-bar-solid.svg";
import GaugeCompo from "./GaugeCompo";
export default {
  name: "CarteCompo",
  components: {
    LMap,
    LTileLayer,
    "l-choropleth-layer": ChoroplethLayer,
    TwitterIcon,
    StatIcon,
    GaugeCompo
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
      currentFilter: "commune",
      colorScale: ["90d0e7", "7baee9", "6270de"],
      nbTweetMax: 10,
      value: {},
      extraValues: [],
      mapOptions: {
        attributionControl: false
      },
      currentStrokeColor: "333333"
    };
  },
  created() {
    // Update à chaque click
    window.addEventListener("click", () => {
      this.updateZoom();
    });
    // Ajout du listener sur le bouton de géolocalisation
    window.addEventListener("load", () => {
      document
        .getElementsByClassName("getPosition")[0]
        .addEventListener("click", () => {
          this.getCurrentLocation();
        });
      this.updateZoom();
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
      // Si on passe en commune on regarde où le user a cliqué
      if (this.$store.state.currentFilter === "departement") {
        this.getCommunesFromEvent(event.latlng.lat, event.latlng.lng);
      }

      if (this.$store.state.currentFilter === "region") {
        this.centerUpdated([event.latlng.lat, event.latlng.lng]);
        this.$store.state.currentFilter = "departementN";
        this.updateZoom();
      }
    },
    getCommunesFromEvent(lat, lng) {
      axios
        .get(
          "https://api.opencagedata.com/geocode/v1/json?q=" +
            lat +
            "+" +
            lng +
            "&key=6cb782be82c646cfb05d9471b7ca2961"
        )
        .then(res => {
          this.centerUpdated([lat, lng]);
          this.getDataFromGeoJson(
            res.data.results[0].components.state_district.toUpperCase(),
            res.data.results[0].components.postcode.substring(0, 2)
          );
        });
    },
    getCurrentLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          position => {
            this.getCommunesFromEvent(
              position.coords.latitude,
              position.coords.longitude
            );
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
    getNewsId() {
      var newsId = "[";
      var nb = 0;
      this.$store.state.chipsList.forEach(n => {
        nb += 1;
        newsId = newsId + '"' + n._id + '",';
      });
      if (nb > 0) {
        newsId = newsId.substr(0, newsId.length - 1);
      }
      newsId = newsId + "]";
      return newsId;
    },
    async getDataFromGeoJson(nomDept, codeDept) {
      this.nbTweetMax = 0; //Reset du nb de TweetMax
      var str = "";
      if (codeDept == 20) {
        str = 'nom_dept: "' + nomDept + '"';
      } else {
        str = 'code_dept: "' + codeDept + '"';
      }
      var newsId = this.getNewsId();
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
            topHashtagsFromAllCitiesInOneDepartement(depCode:"${codeDept}", newsId: ${newsId}) {
              _id
              hashtags {
                hashtag
                count
              }
            }
          }
        `
      });
      let resApolloDeb = await this.$apollo.query({
        query: gql`
          query {
            tweetsPerDayFromAllCitiesInOneDepartement(depCode: "${codeDept}", newsId: ${newsId}) {_id, count}
          }
        `
      });
      //Contient l'évolution du nombre de tweet par rapport au dernière 24h
      let resApolloEvolveNbTweet = await this.$apollo.query({
        query: gql`
          query {
           differenceOfNumberOfTweetsPerDayFromAllCitiesInOneDepartement(depCode:"${codeDept}"){zoneNumber,percentage}

          }
        `
      });

      console.log(resApollo);
      console.log(resApolloHash);
      console.log(resApolloDeb);
      console.log(resApolloEvolveNbTweet);

      this.communes = resApollo.data.communes;
      this.communesInfo = [];

      /*
       * Formatage des données recus pour insertion dans la carte leaflet
       */
      resApollo.data.communes.forEach(element => {
        element.properties.rien = "";
        element.properties.hashtags = [];
        element.properties.hashtags1 = "";
        element.properties.debit = 0;
        element.properties.evolveNbTweetDay = 0;

        /* ==============================================================
            Gestion des arrondissements ( Paris, Lyon, Marseille)
         =============================================================== */
        let codeInsee = codeDept + element.properties.code;
        //Paris
        if (
          codeDept + element.properties.code > 75100 &&
          codeDept + element.properties.code < 75121
        ) {
          codeInsee = "75056";
        }
        //Lyon
        else if (
          codeDept + element.properties.code > 69380 &&
          codeDept + element.properties.code < 69390
        ) {
          codeInsee = "69123";
        }
        //Marseille
        else if (
          codeDept + element.properties.code > 13200 &&
          codeDept + element.properties.code < 13217
        ) {
          codeInsee = "13055";
        }

        /* ================================
           Gestion des hastags par commune
         ================================= */

        //Cherche si il existe des hastags pour la commune
        var has = resApolloHash.data.topHashtagsFromAllCitiesInOneDepartement
          .map(function(x) {
            return x._id;
          })
          .indexOf(codeInsee);
        //Si has != -1 ( si trouvé ) affecte le tableau des hastags sinon tableau vide
        element.properties.hashtags =
          has !== undefined && has !== -1
            ? resApolloHash.data.topHashtagsFromAllCitiesInOneDepartement[has]
                .hashtags
            : [];
        //Tri les hastags par ordre croissant
        element.properties.hashtags.sort((a, b) => {
          return b.count - a.count;
        });
        /* =================================================
           Gestion du nombre de tweet par jour  par commune
         =================================================== */
        has = resApolloDeb.data.tweetsPerDayFromAllCitiesInOneDepartement
          .map(function(x) {
            return x._id;
          })
          .indexOf(codeInsee);
        //Si has != -1 ( si trouvé ) affecte le tableau des hastags sinon tableau vide
        element.properties.debit =
          has !== undefined && has !== -1
            ? resApolloDeb.data.tweetsPerDayFromAllCitiesInOneDepartement[has]
                .count
            : [];
        /* ===============================================================================
              Gestion du pourcentage d'évolution des tweets par rapport au jour n-1
         ================================================================================= */
        has = resApolloEvolveNbTweet.data.differenceOfNumberOfTweetsPerDayFromAllCitiesInOneDepartement
          .map(function(x) {
            return x.zoneNumber;
          })
          .indexOf(codeInsee);
        //Si has != -1 ( si trouvé ) affecte le tableau des hastags sinon 0
        element.properties.evolveNbTweetDay =
          has !== undefined && has !== -1
            ? resApolloEvolveNbTweet.data.differenceOfNumberOfTweetsPerDayFromAllCitiesInOneDepartement[
                has
              ].percentage.toFixed(2)
            : 0;

        //Si il existe des hastags pour cette commune ,initialise hastag1
        if (element.properties.hashtags.length > 0) {
          element.properties.hashtags1 = element.properties.hashtags[0].hashtag;
        }
        //Cherche le nombre de tweetMax pour la zone
        this.nbTweetMax =
          element.properties.debit > this.nbTweetMax
            ? element.properties.debit
            : this.nbTweetMax;
        //Ajoute la commune au tableau contenant toutes les informations
        this.communesInfo.push(element.properties);
      });

      this.$store.state.currentFilter = "communeN"; //Mise à jour du store
      this.updateZoom();
    },

    updateZoom() {
      switch (this.$store.state.currentFilter) {
        case "regionN":
          this.zoomUpdated(6);
          this.getRegDataFromGeoJson();
          this.value.key = "debit";
          this.value.metric = "";
          this.extraValues = [
            {
              key: "population"
            },
            {
              key: "superficie"
            },
            {
              key: "hashtags"
            },
            {
              key: "debit"
            },
            {
              key: "evolveNbTweetDay"
            }
          ];
          this.$store.state.currentFilter = "region";
          break;
        case "departementN":
          this.zoomUpdated(7);
          this.getDeptDataFromGeoJson();
          this.value.key = "debit";
          this.value.metric = "";
          this.extraValues = [
            {
              key: "population"
            },
            {
              key: "superficie"
            },
            {
              key: "hashtags"
            },
            {
              key: "debit"
            },
            {
              key: "evolveNbTweetDay"
            }
          ];
          this.$store.state.currentFilter = "departement";
          break;
        case "communeN":
          this.zoomUpdated(10);
          this.value.key = "debit";
          this.value.metric = "hab";
          this.extraValues = [
            {
              key: "population"
            },
            {
              key: "superficie"
            },
            {
              key: "hashtags"
            },
            {
              key: "debit"
            },
            {
              key: "evolveNbTweetDay"
            }
          ];
          this.$store.state.currentFilter = "commune";
          break;
      }
    },
    async getRegDataFromGeoJson() {
      this.nbTweetMax = 0; //Reset du nb de TweetMax
      var newsId = this.getNewsId();
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

      let resApolloHash = await this.$apollo.query({
        query: gql`
          query {
            topHashtagsFromAllRegions(newsId: ${newsId}) {
              _id
              hashtags {
                hashtag
                count
              }
            }
          }
        `
      });
      let resApolloDeb = await this.$apollo.query({
        query: gql`
          query {
            tweetsPerDayFromAllRegions(newsId: ${newsId}) {
              _id
              count
            }
          }
        `
      });
      //Contient l'évolution du nombre de tweet par rapport au dernière 24h
      let resApolloEvolveNbTweet = await this.$apollo.query({
        query: gql`
          query {
            differenceOfNumberOfTweetsPerDayFromAllRegions(newsId: []) {
              zoneNumber
              percentage
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
        element.properties.hashtags = [];
        element.properties.hashtags1 = "";
        element.properties.debit = 0;
        this.communesInfo.push(element.properties);
      });

      resApolloDeb.data.tweetsPerDayFromAllRegions.forEach(r => {
        var com = this.communes.find(c => {
          return c.properties.code === r._id;
        });
        if (com) {
          com.properties.debit = r.count;
        }
      });

      resApolloHash.data.topHashtagsFromAllRegions.forEach(topHash => {
        var com = this.communes.find(c => {
          return c.properties.code === topHash._id;
        });
        if (com) {
          com.properties.hashtags = topHash.hashtags;
        }
      });

      //Ajoute le pourcentage d'évolution des tweets par rapport au jour n-1
      resApolloEvolveNbTweet.data.differenceOfNumberOfTweetsPerDayFromAllRegions.forEach(
        reg => {
          var com = this.communes.find(c => {
            return c.properties.code === reg.zoneNumber;
          });
          if (com) {
            com.properties.evolveNbTweetDay = reg.percentage.toFixed(2);
          }
        }
      );

      this.communes.forEach(c => {
        if (c.properties.hashtags.size !== 0) {
          c.properties.hashtags.sort((a, b) => {
            return b.count - a.count;
          });
          if (c.properties.hashtags.length > 0)
            c.properties.hashtags1 = c.properties.hashtags[0].hashtag;
        }
        //Cherche le nombre de tweetMax pour la zone
        this.nbTweetMax =
          c.properties.debit > this.nbTweetMax
            ? c.properties.debit
            : this.nbTweetMax;
      });
    },
    async getDeptDataFromGeoJson() {
      this.nbTweetMax = 0; //Reset du nb de TweetMax
      var newsId = this.getNewsId();
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

      let resApolloHash = await this.$apollo.query({
        query: gql`
          query {
            topHashtagsFromAllDepartements(newsId: ${newsId}) {
              _id
              hashtags {
                hashtag
                count
              }
            }
          }
        `
      });
      let resApolloDeb = await this.$apollo.query({
        query: gql`
          query {
            tweetsPerDayFromAllDepartements(newsId: ${newsId}) {
              _id
              count
            }
          }
        `
      });
      //Contient l'évolution du nombre de tweet par rapport au dernière 24h
      let resApolloEvolveNbTweet = await this.$apollo.query({
        query: gql`
          query {
            differenceOfNumberOfTweetsPerDayFromAllDepartements(newsId: []) {
              zoneNumber
              percentage
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
        element.properties.hashtags = [];
        element.properties.hashtags1 = "";
        element.properties.debit = 0;
        this.communesInfo.push(element.properties);
      });

      resApolloDeb.data.tweetsPerDayFromAllDepartements.forEach(r => {
        var com = this.communes.find(c => {
          return c.properties.code === r._id;
        });
        if (com) {
          com.properties.debit = r.count;
        }
      });

      resApolloHash.data.topHashtagsFromAllDepartements.forEach(topHash => {
        var com = this.communes.find(c => {
          return c.properties.code === topHash._id;
        });
        if (com) {
          com.properties.hashtags = topHash.hashtags;
        }
      });
      //Ajoute le pourcentage d'évolution des tweets par rapport au jour n-1
      resApolloEvolveNbTweet.data.differenceOfNumberOfTweetsPerDayFromAllDepartements.forEach(
        reg => {
          var com = this.communes.find(c => {
            return c.properties.code === reg.zoneNumber;
          });
          if (com) {
            com.properties.evolveNbTweetDay = reg.percentage.toFixed(2);
          }
        }
      );

      this.communes.forEach(c => {
        if (c.properties.hashtags.size !== 0) {
          c.properties.hashtags.sort((a, b) => {
            return b.count - a.count;
          });
          if (c.properties.hashtags.length > 0)
            c.properties.hashtags1 = c.properties.hashtags[0].hashtag;
        }
        //Cherche le nombre de tweetMax pour la zone
        this.nbTweetMax =
          c.properties.debit > this.nbTweetMax
            ? c.properties.debit
            : this.nbTweetMax;
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
.twitter-icon,
.chart-bar-icon {
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