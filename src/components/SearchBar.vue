<template>
  <div>
    <div class="wrapperNav mx-auto">
      <b-navbar toggleable="sm" type="light" variant="light" class="col-lg-8 col-12">
        <div class="row d-flex align-items-center w-100 flex-wrap">
          <div class="col-lg-8 col-6 d-flex align-items-center searchPart">
            <div class="col-1 mr-3">
              <div class="circle d-flex align-items-center justify-content-center">
                <i class="fas fa-search btn-search mx-2"></i>
              </div>
            </div>
            <input id="searchBar" class="searchbar w-75" type="text" placeholder="Search..." />
          </div>
          <div class="col-1 iconLink">
            <i class="far fa-list-alt"></i>
          </div>

          <div class="col-1 iconLink">
            <i class="far fa-compass"></i>
          </div>
          <div class="col-1 iconLink">
            <i class="far fa-map"></i>
          </div>
          <div class="col-1 iconLink">
            <img alt="Vue logo" src="../assets/logo.png" width="40" />
          </div>
        </div>
      </b-navbar>
    </div>

    <div class="wrapper" :class="wrapperExp?'wrapperExpand':''">
      <div class="menuWrapper" @click="wrapperExp = !wrapperExp">
        <i class="fas fa-layer-group"></i>
      </div>
      <div class="col" v-if="wrapperExp">
        <div class="row">
          <i class="far fa-compass"></i>
          <h6>Région</h6>
        </div>
        <div class="row">
          <i class="far fa-compass"></i>
          <h6>
            Dépar-
            <br />tement
          </h6>
        </div>
        <div class="row">
          <i class="far fa-compass"></i>
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
      wrapperExp: false
    };
  },
  mounted() {},
  created() {},
  methods: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapperNav {
  width: 100vw;
  display: flex;
  justify-content: center;
}
nav {
  position: absolute;

  top: 3%;
  z-index: 999;
  border-radius: 25em;
  cursor: pointer;
  box-shadow: 0 1.25rem 1.562rem -1.25rem rgba(0, 0, 0, 0.4);
  border: 1px solid #eaf1f3;
  background-color: #fff !important;
  padding: 0;
}
.searchbar {
  float: right;
  color: #333;
  padding: 6px 10px;
  width: 100%;
  border: none;
  margin-top: 1px;
  margin-right: 8px;
  font-family: "Segoe UI Light", "Segoe UI", "Segoe", Tahoma, Helvetica, Arial,
    sans-serif;
  font-size: 1em;
  font-weight: bold;
  border-bottom: #798186c2 solid 2px;
  transition: 0.3s;
}

.searchbar::placeholder {
  color: #798186c2;
  font-family: "Segoe UI Light", "Segoe UI", "Segoe", Tahoma, Helvetica, Arial,
    sans-serif;
  font-size: 1em;
  font-weight: bold;
  /* transition: 0.2s; */
}
.circle {
  width: 40px;
  height: 40px;
  background-color: #6892fc;
  border-radius: 50%;
}
.searchPart,
.iconLink {
  padding: 0.75rem 1rem;
}
.btn-search {
  color: #fff;
  font-size: 1.25em;
}

.iconLink {
  border-left: 3px solid #eaf1f3;
  font-size: 26px;
  color: #798186c2;
  height: 74px;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Filter */
.wrapper {
  background: #fff;
  width: 73px;
  height: auto;
  /*height: 385px;*/
  border-radius: 25em !important;
  overflow: hidden;
  position: absolute;
  right: 1%;
  top: 3%;
  z-index: 999;
  box-shadow: 0 3px 5px -1px rgba(0, 0, 0, 0.2),
    0 6px 10px 0 rgba(0, 0, 0, 0.14), 0 1px 18px 0 rgba(0, 0, 0, 0.12);
  max-height: 72px;
  transition: max-height 0.15s ease-out;
  cursor: pointer;
}
.wrapperExpand {
  width: 73px;
  height: auto;
  transition: all 20s ease-in-out;
  max-height: 500px;
  transition: max-height 0.25s ease-in;
}
.menuWrapper {
  width: 73px;
  height: 73px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.menuWrapper .fas {
  padding: 23px 0;
  width: 73px;
  height: 73px;
  color: #798186c2;
  font-size: 26px;
}
.wrapper .row {
  padding: 0px;
  border-top: 2px solid #798186c2;
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 80px;
}
.wrapper h6 {
  font-size: 0.75rem;
}
.fa-compass {
  color: #798186c2;
  font-size: 26px;
}
</style>
