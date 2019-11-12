<template>
  <div>
    <!-- Sidebar -->
    <aside :class="{ clodedSideNav: openSideNav}">
      <ul class="list-unstyled components">
        <li class="asideLink d-flex align-items-center border-bottom title-link">
          <a href="/">Polytweet</a>
        </li>
        <li class="asideLink d-flex align-items-center">
          <i class="far fa-list-alt"></i>
          <span class="ml-4">Fuck</span>
        </li>
        <li class="asideLink d-flex align-items-center">
          <i class="far fa-compass"></i>
          <span class="ml-4">Le</span>
        </li>
        <li class="asideLink d-flex align-items-center">
          <i class="far fa-map"></i>
          <span class="ml-4">C</span>
        </li>
        <li class="asideLink d-flex align-items-center">
          <i class="fas fa-chart-line"></i>
          <span class="ml-4">S</span>
        </li>
        <li class="asideLink d-flex align-items-center">
          <i class="far fa-heart"></i>
          <span class="ml-4">S</span>
        </li>
      </ul>
    </aside>

    <!-- Overlay -->
    <div class="overlay" :class="{ active: !openSideNav}" @click="openSideNav = !openSideNav"></div>

    <!-- Search Nav -->
    <div class="searchNavContainer mx-auto">
      <b-navbar
        toggleable="sm"
        type="light"
        variant="light"
        class="col-lg-6 col-md-9 col-10 searchNav"
      >
        <div class="row align-items-center w-100">
          <div
            class="sidebarButton col-3 col-md-2 justify-content-center d-flex align-items-center"
            @click="toggleNav()"
          >
            <i class="fas fa-bars"></i>
          </div>
          <div class="col-9 col-md-10 d-flex align-items-center inputContainer">
            <input
              id="searchBar"
              class="inputSearchbar w-100"
              type="text"
              placeholder="Entrez votre recherche"
            />
            <div class="col-2 col-md-1">
              <div class="circle d-flex align-items-center justify-content-center">
                <i class="fas fa-search btn-search mx-2"></i>
              </div>
            </div>
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
          :class="{ filterActive: currentFilter=='region'}"
          @click="currentFilter = 'regionN'"
        >
          <i class="far fa-circle" v-if="currentFilter!='region'"></i>
          <i class="far fa-check-circle" v-if="currentFilter=='region'"></i>
          <h6>Région</h6>
        </div>
        <div
          class="row"
          :class="{ filterActive: currentFilter=='departement'}"
          @click="currentFilter = 'departementN'"
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
          @click="currentFilter = 'communeN'"
        >
          <i class="far fa-circle" v-if="currentFilter!='commune'"></i>
          <i class="far fa-check-circle" v-if="currentFilter=='commune'"></i>
          <h6>Commune</h6>
        </div>
      </div>
    </div>
  </div>
</template>


 

<script>
export default {
  name: "SearchBar",

  data() {
    return {
      wrapperExp: false,
      openSideNav: true,
    };
  },
  computed: {
    currentFilter: {
      get () {
        return this.$store.state.currentFilter
      },
      set (newV) {
        this.$store.state.currentFilter = newV
      }
    }
  },
  watch: {
    openSideNav: function() {
      this.$emit("sideNavState", this.openSideNav);
    }
  },
  mounted() {},
  created() {},
  methods: {
    toggleNav() {
      this.openSideNav = !this.openSideNav;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* =============================
            SIDEBAR
 ============================= */
aside {
  display: block;
  min-width: 250px;
  max-width: 250px;
  background: #fff;
  color: #798186c2;
  transition: all 0.3s;
  height: 100vh;
  position: absolute;
  z-index: 999;
}
aside.clodedSideNav {
  margin-left: -260px;
}

/*Title ( Polytweet)*/
aside li.title-link {
  height: 95px !important;
  /*  margin-left: 15px;
  margin-right: 10px;
  margin-bottom: 10px; */
  justify-content: center;
  padding-left: 0px !important;
}
aside li.title-link a {
  text-decoration: none;
  color: #6892fc;
  font-family: "Airbnb Cereal App Black";
  font-size: 24px;
  /*  margin-left: -15px; */
}

/*Other Links*/
.asideLink {
  height: 50px;
  padding-left: 15%;
  cursor: pointer;
}
.asideLink i {
  font-size: 20px;
  color: #798186c2;
}
.asideLink.border-bottom {
  border-bottom: 1px solid #e8eaed;
}
.asideLink:not(.title-link):hover {
  background-color: #e8eaed;
}

/*Overlay*/
.overlay {
  display: none;
  position: fixed;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.7);
  z-index: 997;
  opacity: 0;
  transition: all 0.5s ease-in-out;
}
.overlay.active {
  display: block;
  opacity: 1;
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
  border-radius: 25em;
  cursor: pointer;
  box-shadow: 0 1.25rem 1.562rem -1.25rem rgba(0, 0, 0, 0.4);
  border: 1px solid #eaf1f3;
  background-color: #fff !important;
  padding: 0.75rem 1rem;
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
  border-bottom: #798186c2 solid 2px;
  transition: 0.3s;
}
.inputSearchbar::placeholder {
  color: #798186c2;
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

/* =============================
           FILTRES
 ============================= */
.filterSideNav {
  background: #fff;
  width: 66px;
  height: auto;
  border-radius: 25em !important;
  overflow: hidden;
  position: absolute;
  right: 1%;
  top: 3%;
  z-index: 510;
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
.filterSideNav i {
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
    top: 15%;
  }
  .filterItemContainer {
    width: 50px;
    height: 50px;
  }
  .filterSideNav h6 {
    word-break: break-word;
    font-size: 0.6rem;
  }
}
</style>
