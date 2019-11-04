<template>
  <div>
    <SearchBar @filterData="updateFilter" @sideNavState="updateSideState"></SearchBar>
    <CarteCompo :currentFilter="childData" :currentSideState="childSideState"></CarteCompo>
  </div>
</template>

<script>
// @ is an alias to /src
import CarteCompo from "@/components/CarteCompo.vue";
import SearchBar from "@/components/SearchBar.vue";

export default {
  name: "Carte",
  components: {
    CarteCompo,
    SearchBar
  },
  data: function() {
    return {
      childData: "",
      childSideState: ""
    };
  },
  methods: {
    updateFilter(variable) {
      this.childData = variable;
    },
    updateSideState(variable) {
      this.childSideState = variable;
    }
  }
};
</script>
