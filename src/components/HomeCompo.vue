<template>
  <div>
    <div class="home">
      <div class="svgHome">
        <img
          class="svgBackground"
          src="../assets/svg/homePage.svg"
          width="100%"
          alt="Background home"
        />
      </div>
      <div class="row">
        <div class="textWrapper col-md-6 col-lg-4">
          <h3>
            Polytweet, la solution pour connaître
            <br />tous les hastags à suivre
          </h3>
          <p
            class="mt-3"
          >Lorem ipsum dolor sit amet consectetur adipisicing elit. Minus officiis enim sit impedit necessitatibus, fuga, obcaecati excepturi eos dolore reiciendis quasi nemo! Commodi asperiores a sapiente. Sunt nam necessitatibus minima.</p>
          <router-link to="/carte">
            <button class="btn btn-primary mt-4">Découvrir l'application</button>
          </router-link>
        </div>
      </div>
    </div>
    <div class="compteur">
      <h1>{{dataCompteur}}</h1>
    </div>
  </div>
  <!-- 
  explication projet
  mention legales
  -->
</template>

<script>
export default {
  name: "HomeCompo",
  data() {
    return {
      dataCompteur: 175302
    };
  },
  props: {},
  methods: {
    regenerateDataCompteur() {
      console.log("regen");
    }
  },
  watch: {
    dataCompteur: () => {
      var digits = ("" + this.dataCompteur).split("");
      console.log(digits);
    }
  },
  mounted() {
    this.regenerateDataCompteur();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Home */
.home,
.compteur {
  height: 100vh;
  width: 100vw;
}
.home {
  display: flex;
  align-items: center;
  max-width: 100%;
  overflow: hidden;
  z-index: -1;
}
.svgHome {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0px;
  left: 0px;
  z-index: -1;
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

/* Compteur */
.compteur {
  background-color: #dedede;
  display: flex;
  align-items: center;
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
