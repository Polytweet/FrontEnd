<template >
  <div>
    <div class="container">
      <h1 class="titlePage" v-scroll-reveal>PolyTeam</h1>
      <div class="row d-flex justify-content-center">
        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 500 }">
          <b-card>
            <img :src="images.maxence" alt="Profil image" class="card-img-top" />
            <h4 class="card-title mt-3">Maxence Bessy</h4>
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Front-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 800 }">
          <b-card>
            <img :src="images.marion" alt="Profil image" class="card-img-top" />
            <h4 class="card-title mt-3">Marion Sinaeve</h4>
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Back-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 1000 }">
          <b-card>
            <img :src="images.michael" alt="Profil image" class="card-img-top" />
            <h4 class="card-title mt-3">Michael Bugnone</h4>
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Back-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 1200 }">
          <b-card>
            <img :src="images.aurian" alt="Profil image" class="card-img-top" />
            <h4 class="card-title mt-3">Aurian Durand</h4>
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Back-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 1400 }">
          <b-card>
            <img :src="images.arnaud" alt="Profil image" class="card-img-top" />
            <h4 class="card-title mt-3">Arnaud Volpi</h4>
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Front-End</b-card-text>
          </b-card>
        </div>
      </div>
    </div>
  </div>
</template>


 

<script>
export default {
  name: "TeamCompo",
  data() {
    return {
      images: {
        maxence: require("@/assets/img/team/maxence.jpeg"),
        marion: require("@/assets/img/team/marion.jpeg"),
        aurian: require("@/assets/img/team/aurian.jpeg"),
        michael: require("@/assets/img/team/michael.jpeg"),
        arnaud: require("@/assets/img/team/arnaud.jpeg")
      }
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Team*/
h1.titlePage {
  padding-top: 80px;
}
h1::after {
  content: "";
  display: block;
  margin: 10px auto;
  margin-bottom: 5%;
  width: 10%;
  height: 5px;
  background: #333;
  transition: width 0.5s;
}
.card {
  background: #fff;
  border-radius: 3px;
  transition: 300ms;
  margin: 10px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
  /*   box-shadow: 0 2px 6px 0 rgba(47, 83, 151, 0.1);
 */
}
.card:hover {
  /* box-shadow: 0 0 20px 9px rgba(0, 0, 0, 0.03); */
}
img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  box-shadow: 0 2px 6px 0 rgba(47, 83, 151, 0.1);
  margin-top: 15px;
  margin-bottom: -5px;
}
.small.text-muted {
  border: 1px solid;
  border-radius: 25rem;
  width: 30%;
}
@media screen and (max-width: 719px) {
  .team {
    padding-top: 75px;
  }
}
</style>
