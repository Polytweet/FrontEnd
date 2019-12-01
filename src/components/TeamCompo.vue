<template >
  <div>
    <div class="container">
      <h1 class="titlePage" v-scroll-reveal>PolyTeam</h1>
      <div class="row d-flex justify-content-center">
        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 500 }">
          <b-card
            title="Maxence Bessy"
            img-src="https://media.licdn.com/dms/image/C4D03AQEf4yZK3TcwQw/profile-displayphoto-shrink_200_200/0?e=1575504000&v=beta&t=_PWsLSaOECNL9dSjMDXSqy7hLDBVzRxSQkok3-FfMU0"
            img-alt="Profil image"
            img-top
          >
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Front-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 800 }">
          <b-card
            title="Marion Sinaeve"
            img-src="https://media.licdn.com/dms/image/C4D03AQH6rABzypU6Gw/profile-displayphoto-shrink_200_200/0?e=1575504000&v=beta&t=6G-nIwlXF8VyFAw5wWUnG1gZtFW020pNcsreqSKPRs8"
            img-alt="Profil image"
            img-top
          >
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Back-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 1000 }">
          <b-card
            title="Michael Bugnone"
            img-src="data:image/jpeg;base64,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"
            img-alt="Profil image"
            img-top
          >
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Back-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 1200 }">
          <b-card
            title="Aurian Durand"
            img-src="https://media.licdn.com/dms/image/C4D03AQG7FA-MeGgGHQ/profile-displayphoto-shrink_200_200/0?e=1575504000&v=beta&t=vFx8yuqqGl45mtVwuEuhFbhvglq-240aPfwZU9TFejA"
            img-alt="Profil image"
            img-top
          >
            <b-card-text>This card has supporting text below as a natural lead-in to additional content.</b-card-text>
            <b-card-text class="small text-muted mx-auto">Back-End</b-card-text>
          </b-card>
        </div>

        <div class="col-12 col-lg-4" v-scroll-reveal="{ delay: 1400 }">
          <b-card
            title="Arnaud Volpi"
            img-src="https://media.licdn.com/dms/image/C4E03AQHjExQrlE7GNA/profile-displayphoto-shrink_200_200/0?e=1575504000&v=beta&t=1EgQ7s_WnE-q9rxZbUHVXnCp0D34Rp5YamABillId-U"
            img-alt="Profil image"
            img-top
          >
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
  mounted() {},
  created() {},
  methods: {}
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
