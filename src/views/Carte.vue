<template>
  <div>
    <SearchBar></SearchBar>
    <CarteCompo></CarteCompo>
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
  }
};
</script>
