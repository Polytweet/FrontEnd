<template>
  <div class="home">
    <div class="svgHome">
      <HomeSvg class="svgBackground" width="100%" alt="Background home" />
    </div>
    <div class="row">
      <div class="textWrapper col-md-6 col-lg-4">
        <h3>
          Polytweet, la solution pour connaître
          <br />tous les hastags à suivre
        </h3>
        <p class="mt-3">
          Avec Polytweet, suivez en temps réel l'évolution des derniers tweets les plus partagés en France ou tout simplement autour de chez vous .
          Et coupler les avec les actualités françaises pour visualiser les tweets les plus partagés du moment.
        </p>
        <router-link to="/carte">
          <button class="btn btn-primary mt-4">Découvrir l'application</button>
        </router-link>
      </div>
    </div>
  </div>
  <!-- 
  explication projet
  mention legales
  -->
</template>

<script>
import HomeSvg from "@/assets/svg/homePage2-01.svg";
export default {
  name: "HomeCompo",
  components: { HomeSvg },
  props: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Home */
.home {
  height: 100vh;
  width: 100vw;
}
.home {
  display: flex;
  align-items: center;
  max-width: 100%;
  z-index: -1;
}
.svgHome {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0px;
  left: 0px;
  z-index: -1;
  overflow: hidden;
}
.svgBackground {
  fill: white;
}

/*Color Svg after npm i svg Loader*/
.cls-6 {
  fill: #ff6066 !important;
}
.cls-7 {
  fill: #ffe340 !important;
}
.cls-8 {
  fill: #2d2d2c !important;
}
.cls-13 {
  fill: #273b82 !important;
}
.cls-14,
.cls-17 {
  fill: #568bff !important;
}

/*Text*/
.textWrapper {
  text-align: left;
  margin-left: 75px;
}

@media screen and (max-width: 1000px) {
  img.svgBackground {
    width: 155%;
    position: absolute;
    top: 0px;
    right: -15px;
  }
  .textWrapper {
    margin-left: 15px;
  }
}

/*Mobile*/
@media screen and (max-width: 719px) {
  /* Home */
  .textWrapper {
    position: absolute;
    bottom: 10%;
    margin-left: 15px;
  }
  .svgHome {
    visibility: visible;
  }
  img.svgBackground {
    width: 250%;
    position: absolute;
    top: 0px;
    right: -350px;
  }
}
</style>
