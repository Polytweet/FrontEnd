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
              class="inputSearchbar effect-focus-input w-100"
              type="text"
              placeholder="Choisissez une actualité"
              @focus="inputSearchFocus = true"
              v-model="filter"
            />
            <span class="focus-bg"></span>
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
                <span class="mx-auto">{{chips.title.split(' ').slice(0,4).join(' ')}}</span>
                <CloseIcon
                  class="fas float-right ml-1 text-right"
                  v-on:click="removeToChipsList(chips)"
                  height="20"
                />
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

            <table class="d-flex listWrap table table-striped">
              <tbody>
                <tr v-for="data in getNews" v-bind:key="data._id" class="w-100">
                  <td
                    class="newsContent d-flex justify-content-center align-items-center"
                    :disabled="data.chipsSelected==true"
                    v-bind:class="{ chipSelected: data.chipsSelected==true }"
                    v-on:click="addToChipsList(data)"
                  >{{data.title}}</td>
                </tr>
              </tbody>
            </table>

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
        <LayerGroupIcon class="fas icon" />
      </div>
      <div class="col" v-if="wrapperExp">
        <div
          class="row"
          :class="{ filterActive: currentFilter=='region'}"
          @click="currentFilter = 'regionN'"
        >
          <CircleIcon class="fas icon" v-if="currentFilter!='region'" />
          <CircleCheckedIcon class="fas iconChecked" v-if="currentFilter=='region'" />
          <h6>Régions</h6>
        </div>
        <div
          class="row"
          :class="{ filterActive: currentFilter=='departement'}"
          @click="currentFilter = 'departementN'"
        >
          <CircleIcon class="fas icon" v-if="currentFilter!='departement'" />
          <CircleCheckedIcon class="fas iconChecked" v-if="currentFilter=='departement'" />
          <h6>
            Dépar-
            <br />tement
          </h6>
        </div>
        <div
          class="row"
          :class="{ filterActive: currentFilter=='commune'}"
          @click="currentFilter = 'communeN'"
        >
          <CircleIcon class="fas icon" v-if="currentFilter!='commune'" />
          <CircleCheckedIcon class="fas iconChecked" v-if="currentFilter=='commune'" />
          <h6>Commune</h6>
        </div>
      </div>
    </div>

    <!-- Position -->
    <div class="getPosition d-flex justify-content-center align-items-center">
      <GPSIcon class="fas icon" />
    </div>
  </div>
</template>


 

<script>
import gql from "graphql-tag";
import GPSIcon from "@/assets/svg/gps_fixed-24px.svg";
import LayerGroupIcon from "@/assets/svg/layer-group-solid.svg";
import CircleIcon from "@/assets/svg/circle-regular.svg";
import CircleCheckedIcon from "@/assets/svg/check-circle-regular.svg";
import CloseIcon from "@/assets/svg/times-circle-solid.svg";

export default {
  name: "SearchBar",
  components: {
    GPSIcon,
    LayerGroupIcon,
    CircleIcon,
    CircleCheckedIcon,
    CloseIcon
  },
  data() {
    return {
      wrapperExp: false,
      inputSearchFocus: false,
      chipsList: [],
      filter: "",
      news: []
    };
  },
  created() {
    this.getDataFromNews();
  },
  methods: {
    //Ajoute une news au filtres (chips)
    addToChipsList(data) {
      if (data.chipsSelected != true) {
        this.$store.state.chipsList.push(data);
        this.chipsList.push(data);
        this.news[this.news.findIndex(x => x._id === data._id)][
          "chipsSelected"
        ] = true;
      }
    },
    //On click icone croix -> chercher l'object et le retire de la list des chips
    removeToChipsList(data) {
      this.news[this.news.findIndex(x => x._id === data._id)][
        "chipsSelected"
      ] = false;
      this.$store.state.chipsList.splice(
        this.$store.state.chipsList
          .map(function(e) {
            return e.title;
          })
          .indexOf(data.title),
        1
      );
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
      this.news = resApollo.data.news;
    }
  },
  computed: {
    currentFilter: {
      get() {
        return this.$store.state.currentFilter;
      },
      set(newV) {
        this.$store.state.currentFilter = newV;
      }
    },
    getNews() {
      var news = this.news.filter(player => {
        return player.title.toLowerCase().includes(this.filter.toLowerCase());
      });
      return news;
    },
    getChipsList() {
      return this.$store.state.chipsList;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.effect-focus-input ~ .focus-bg {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #ededed;
  opacity: 0;
  transition: 0.5s;
  z-index: -1;
}
.effect-focus-input:focus ~ .focus-bg {
  transition: 0.5s;
  opacity: 1;
}
.effect-focus-input {
  border: 0;
  padding: 7px 15px;
  border: 1px solid #ccc;
  position: relative;
  background: transparent;
}
:focus {
  outline: none;
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
  width: 175px;
  text-align: center;
  border: 2px solid rgba(51, 51, 51, 0.46);
  color: rgba(51, 51, 51, 0.46);
  -ms-flex-align: center;
  align-items: center;
  justify-content: space-between;
  display: inline-flex;
  line-height: 20px;
  padding: 0 12px;
}
.chip .text-left {
  height: 20px;
  overflow: hidden;
  display: flex;
  align-items: flex-start;
  justify-content: center;
}

/* News List */
.listWrap {
  max-height: 55vh;
  overflow: scroll;
  overflow-x: unset;
}
.newsContent {
  color: rgba(51, 51, 51);
  border: none;
  font-weight: 500;
  height: 90px;
}
.chipSelected {
  background-color: #6892fc;
  color: white;
  cursor: not-allowed;
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
  right: 6%;
  top: 3%;
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
  border-top: 2px solid #999fa3;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 80px;
}

.filterSideNav h6 {
  word-break: break-word;
  font-size: 0.75rem;
}
.filterSideNav i,
.getPosition i {
  font-size: 22px;
  color: #999fa3;
}

.fas {
  font-size: 22px;
  color: #999fa3;
  width: 24px;
}

.filterItemContainer {
  width: 66px;
  height: 66px;
}
.filterItemContainer i {
  color: #798186c2;
  font-size: 26px;
}
.filterSideNav .row.filterActive h6,
.filterSideNav .row.filterActive i,
.iconChecked {
  color: #6892fc !important;
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
    right: 1%;
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
