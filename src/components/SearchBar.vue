<template>
  <div>
    <!-- Search Nav -->
    <div class="searchNavContainer mx-auto" @mouseleave="inputSearchFocus = false">
      <b-navbar
        toggleable="sm"
        type="light"
        variant="light"
        class="col-lg-6 col-md-9 col-11 searchNav"
      >
        <div class="row align-items-center w-100 col-12">
          <a
            href="/"
            class="sidebarButton col-2 col-md-2 col-lg-1 d-flex align-items-center justify-content-center"
          >
            <img alt="Vue logo" src="../assets/logo.png" width="40" />
          </a>
          <div class="col-lg-11 col-10 d-flex align-items-center inputContainer">
            <input
              id="searchBar"
              class="inputSearchbar w-100 effect-8"
              type="text"
              placeholder="Choisissez une actualité"
              @focus="inputSearchFocus = true"
              v-model="filter"
            />
            <span class="focus-border">
              <i></i>
            </span>
          </div>
        </div>
        <!-- News Part -->
        <div class="col-12 info-part" v-bind:class="{ searchExpand: inputSearchFocus }">
          <br />
          <!-- NewsSelected -->
          <div class="newsFilter d-flex align-items-start">
            <div class="d-flex flex-wrap w-100">
              <!-- Chips -->
              <div class="chip mt-1" v-for="chips in chipsList" v-bind:key="chips.title">
                <span class="text-left">{{chips.title}}</span>
                <i
                  class="fas fa-times-circle ml-1 float-right text-right"
                  v-on:click="removeToChipsList(chips)"
                ></i>
              </div>
              <!-- /Chips -->
            </div>
          </div>

          <!-- List Header -->
          <div class="newsList mt-4">
            <div class="d-flex justify-content-between">
              <h6 class="mr-3 text-left">News</h6>
              <h6 class="mr-3 text-right">{{getNews.length}} résultats</h6>
            </div>
            <!-- List -->
            <div class="d-flex flex-wrap listWrap">
              <div v-for="data in getNews" v-bind:key="data._id" class="newsItem col-md-6 col-12">
                <button
                  :disabled="data.chipsSelected==true"
                  v-bind:class="{ chipSelected: data.chipsSelected==true }"
                  v-on:click="addToChipsList(data)"
                  class="newsContent d-flex jsutify-content-center align-items-center"
                >{{data.title}}</button>
              </div>
            </div>
            <!-- /List -->
          </div>
        </div>
      </b-navbar>
    </div>

    <!-- Filter SideNav -->
    <div class="filterSideNav" :class="wrapperExp?'filterSideNavExpand':''">
      <div
        class="filterItemContainer d-flex justify-content-center align-items-center"
        @click="wrapperExp = !wrapperExp"
      >
        <i class="fas fa-layer-group"></i>
      </div>
      <div class="col" v-if="wrapperExp">
        <div
          class="row"
          :class="{ filterActive: currentFilter=='france'}"
          @click="currentFilter = 'franceN'"
        >
          <i class="far fa-circle" v-if="currentFilter!='france'"></i>
          <i class="far fa-check-circle" v-if="currentFilter=='france'"></i>
          <h6>France</h6>
        </div>
        <div
          class="row"
          :class="{ filterActive: currentFilter=='departement'}"
          @click="currentFilter = 'departement'"
        >
          <i class="far fa-circle" v-if="currentFilter!='departement'"></i>
          <i class="far fa-check-circle" v-if="currentFilter=='departement'"></i>
          <h6>
            Dépar-
            <br />tement
          </h6>
        </div>
        <div
          class="row"
          :class="{ filterActive: currentFilter=='commune'}"
          @click="currentFilter = 'commune'"
        >
          <i class="far fa-circle" v-if="currentFilter!='commune'"></i>
          <i class="far fa-check-circle" v-if="currentFilter=='commune'"></i>
          <h6>Commune</h6>
        </div>
      </div>
    </div>
    <div class="getPosition d-flex justify-content-center align-items-center">
      <i class="fas fa-map-marker-alt"></i>
    </div>
  </div>
</template>


 

<script>
import json from "../assets/json/news.json";
import gql from "graphql-tag";

export default {
  name: "SearchBar",

  data() {
    return {
      wrapperExp: false,
      currentFilter: "commune",
      inputSearchFocus: false,
      chipsList: [],
      filter: "",
      news: json
    };
  },
  watch: {
    currentFilter: function() {
      this.$emit("filterData", this.currentFilter);
    }
  },

  created() {
    this.getDataFromNews();
  },
  methods: {
    //Ajoute une news au filtres (chips)
    addToChipsList(data) {
      this.chipsList.push(data);
      this.news[this.news.findIndex(x => x._id === data._id)][
        "chipsSelected"
      ] = true;
    },
    //On click icone croix -> chercher l'object et le retire de la list des chips
    removeToChipsList(data) {
      this.news[this.news.findIndex(x => x._id === data._id)][
        "chipsSelected"
      ] = false;
      this.chipsList.splice(
        this.chipsList
          .map(function(e) {
            return e.title;
          })
          .indexOf(data.title),
        1
      );
    },
    async getDataFromNews() {
      let resApollo = await this.$apollo.query({
        query: gql`
          query {
            news {
              _id
              title
              url
            }
          }
        `
      });
      console.log(resApollo);
      this.news = resApollo.data.news;
    }
  },
  computed: {
    getNews() {
      var news = this.news.filter(player => {
        return player.title.toLowerCase().includes(this.filter.toLowerCase());
      });
      return news;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
:focus {
  outline: none;
}
.effect-8,
.effect-9 {
  border: 1px solid #ccc;
  padding: 7px 14px 9px;
  transition: 0.4s;
}

.effect-8 ~ .focus-border:before,
.effect-8 ~ .focus-border:after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  height: 2px;
  background-color: #999fa3;
  border-radius: 10px;
  transition: 0.3s;
}
.effect-8 ~ .focus-border:after {
  top: auto;
  bottom: 0;
  left: auto;
  right: 0;
}
.effect-8 ~ .focus-border i:before,
.effect-8 ~ .focus-border i:after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 2px;
  height: 0;
  background-color: #999fa3;
  transition: 0.4s;
}
.effect-8 ~ .focus-border i:after {
  left: auto;
  right: 0;
  top: auto;
  bottom: 0;
}
.effect-8:focus ~ .focus-border:before,
.effect-8:focus ~ .focus-border:after {
  width: 100%;
  transition: 0.3s;
}
.effect-8:focus ~ .focus-border i:before,
.effect-8:focus ~ .focus-border i:after {
  height: 100%;
  transition: 0.4s;
}

/* =============================
           SEARCH NAV
 ============================= */
.searchNavContainer {
  width: 100vw;
  display: flex;
  justify-content: center;
}
/*SearchNav*/
.searchNav {
  position: absolute;
  top: 3%;
  z-index: 510;
  border-radius: 10px;
  cursor: pointer;
  box-shadow: 0 1.25rem 1.562rem -1.25rem rgba(0, 0, 0, 0.2);
  border: 1px solid #eaf1f3;
  background-color: #fff !important;
  padding: 0.75rem 1rem;
  /*New*/
  flex-wrap: wrap;
}

/*Button who open the sidenav*/
.sidebarButton i {
  font-size: 24px;
  padding: 7px 10px;
  color: #798186c2;
}

/*Input of searchNav*/
.inputSearchbar {
  float: right;
  color: #333;
  padding: 6px 10px;
  width: 100%;
  border: none;
  margin-top: 1px;
  margin-right: 8px;
  font-size: 1em;
  font-weight: bold;
  border-bottom: none;
  transition: 0.3s;
}
.inputSearchbar::placeholder {
  color: #2c3e50;
  font-size: 1em;
  font-weight: bold;
}

/*Button validate search*/
.circle {
  width: 40px;
  height: 40px;
  background-color: #6892fc;
  border-radius: 50%;
}
.btn-search {
  color: #fff;
  font-size: 1.25em;
}

/* News part */
.info-part {
  height: 0px;
  overflow: hidden;
  -moz-transition: height 0.5s;
  -ms-transition: height 0.5s;
  -o-transition: height 0.5s;
  -webkit-transition: height 0.5s;
  transition: height 0.5s;
}
.searchExpand {
  height: 65vh;
  -moz-transition: height 0.5s;
  -ms-transition: height 0.5s;
  -o-transition: height 0.5s;
  -webkit-transition: height 0.5s;
  transition: height 0.5s;
}

/*Chips*/
.chip {
  border-radius: 16px;
  font-size: 14px;
  font-weight: 600;
  height: auto;
  width: 100%;
  border: 2px solid rgba(51, 51, 51, 0.46);
  color: rgba(51, 51, 51, 0.46);
  -ms-flex-align: center;
  align-items: center;
  justify-content: space-between;
  display: inline-flex;
  line-height: 20px;
  padding: 0 12px;
}
.chipSelected {
  background-color: rgb(51, 51, 51, 0.46);
  color: white;
  cursor: not-allowed;
}

/* News List */
.listWrap {
  max-height: 52vh;
  overflow: scroll;
}
.newsItem {
  padding: 5px;
}
.newsContent {
  color: rgba(51, 51, 51);
  border: 1.5px solid rgba(51, 51, 51);
  font-weight: 500;
  height: 75px;
  border-radius: 5px;
  padding: 5px;
}
/* =============================
           FILTRES
 ============================= */
.getPosition {
  background: #fff;
  width: 66px;
  height: 66px;
  border-radius: 25em !important;
  position: absolute;
  right: 1%;
  bottom: 3%;
  z-index: 509;
  box-shadow: 0 3px 5px -1px rgba(0, 0, 0, 0.2),
    0 6px 10px 0 rgba(0, 0, 0, 0.14), 0 1px 18px 0 rgba(0, 0, 0, 0.12);
  cursor: pointer;
}
.filterSideNav {
  background: #fff;
  width: 66px;
  height: auto;
  border-radius: 25em !important;
  overflow: hidden;
  position: absolute;
  right: 1%;
  top: 3%;
  z-index: 509;
  box-shadow: 0 3px 5px -1px rgba(0, 0, 0, 0.2),
    0 6px 10px 0 rgba(0, 0, 0, 0.14), 0 1px 18px 0 rgba(0, 0, 0, 0.12);
  max-height: 72px;
  transition: max-height 0.25s ease-out;
  cursor: pointer;
}
.filterSideNavExpand {
  width: 66px;
  height: auto;
  transition: all 20s ease-in-out;
  max-height: 500px;
  transition: max-height 0.25s ease-in;
}

.filterSideNav .row {
  padding: 0px;
  border-top: 2px solid #798186c2;
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 80px;
}

.filterSideNav h6 {
  word-break: break-word;
  font-size: 0.75rem;
}
.filterSideNav i,
.getPosition i {
  font-size: 22px;
  color: #798186c2;
}

.filterItemContainer {
  width: 66px;
  height: 66px;
}
.filterItemContainer i {
  color: #798186c2;
  font-size: 26px;
}
.filterSideNav .row.filterActive,
.filterSideNav .row.filterActive i {
  color: #6892fc;
}

/* =============================
           RESPONSIVE
 ============================= */
/*Mobile*/
@media (max-width: 576px) {
  .inputContainer,
  .inputSearchbar {
    padding: 0px;
  }
  .filterSideNav {
    width: 50px;
    top: 24%;
  }
  .filterItemContainer,
  .getPosition {
    width: 50px;
    height: 50px;
  }
  .getPosition {
    top: 15%;
  }
  .filterSideNav h6 {
    word-break: break-word;
    font-size: 0.6rem;
  }
  /* News List*/
  .newsContent {
    height: 125px;
  }
}
</style>
