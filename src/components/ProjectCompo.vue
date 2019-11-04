<template >
  <div>
    <h1 class="titlePage" v-scroll-reveal>Polytweet</h1>

    <div class="container-fluid">
      <div class="section-wrapper">
        <!-- This section will reveal itself each time it's scrolled into view -->
        <section v-scroll-reveal="{ delay: 500 }">
          <div class="row align-items-center justify-content-center">
            <div class="col-4 col-md-4 d-none d-sm-block">
              <div class="window mac">
                <div class="title-bar">
                  <div class="close"></div>
                  <div class="minimize"></div>
                  <div class="zoom"></div>
                </div>
                <div class="page">
                  <img src="../assets/img/home.png" class="img-fluid" />
                </div>
              </div>
            </div>
            <div class="col-12 col-md-6">
              <div>
                <h2>Présentation du projet</h2>
              </div>
              <p class="text-left">
                Notre projet utilise les données que l’on peut trouver sur Twitter.
                Le but est de mettre en avant certains thèmes récurrents en fonction
                du lieu sur lequel ils ont été tweeté ainsi qu’en fonction de l’actualité
                que l’on peut retrouver dans ce lieu. Pour cela, nous récupérerons les
                tweets des dernières 24h que nous stockerons dans une base de
                données. Avant de les ajouter à la base de données, on s’assurera de
                ne garder que des tweets publiés depuis la France. Nous allons réaliser
                un site internet sur lequel les visiteurs pourront, par le biais d’une carte
                interactive de la France, obtenir des informations comme par exemple
                les différents hashtags les plus récurrents sur une zone géographique
                précise.
              </p>
            </div>
          </div>
        </section>
      </div>
      <div class="section-wrapper">
        <!-- This section will reveal itself each time it's scrolled into view -->
        <section v-scroll-reveal="{ delay: 300 }">
          <div class="row align-items-center justify-content-center">
            <div class="col-12 col-md-6">
              <div>
                <h2>Présentation des technologies</h2>
              </div>
              <p class="text-left">
                Afin de réaliser notre projet et de le rendre facilement accessible,
                nous avons décidé de créer une application web. Cette application
                sera accompagnée d’un serveur et d’une base de données pour le
                stockage d’informations et l'intelligence du système d’information.
                L’architecture que nous avons choisie est un MVC (Model, View,
                Controller). Le Controller va être développé avec Node.JS et le
                framework Apollo GraphQL (nous permettant de créer une API avec
                GraphQL, plus performante qu’une API REST dans certains cas et plus
                modulable). Le Model quant à lui va être développé avec MongoDB en
                raison de la quantité de données que l’on va traiter et la View sera
                créée avec Vue.JS pour avoir une application moderne et responsive.
              </p>
            </div>
            <div class="col-4 col-md-4 d-none d-sm-block">
              <div class="window mac">
                <div class="title-bar">
                  <div class="close"></div>
                  <div class="minimize"></div>
                  <div class="zoom"></div>
                </div>
                <div class="page">
                  <img
                    src="https://images.pexels.com/photos/924675/pexels-photo-924675.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
                    class="img-fluid"
                  />
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>


 

<script>
export default {
  name: "ProjectCompo",
  mounted() {},
  created() {},
  methods: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1.titlePage {
  padding-top: 80px;
}
h1.titlePage::after {
  content: "";
  display: block;
  margin: 10px auto;
  margin-bottom: 5%;
  width: 10%;
  height: 5px;
  background: #333;
  transition: width 0.5s;
}
h2::after {
  content: "";
  display: block;
  margin: 10px auto;
  margin-bottom: 5%;
  width: 25%;
  height: 5px;
  background: #333;
  transition: width 0.5s;
}
.section-wrapper {
  min-height: 40vh;
  margin-top: 10%;
  margin-bottom: 18%;
}
section {
  display: flex;
  justify-content: center;
  flex-direction: column;
}

/* =================
    MAC WINDOW
 ================= */
.window {
  width: 90%;
  background: #eee;
  display: block;
  margin: 0 auto;
  top: 20%;
  max-height: 80%;
  min-height: 100px;
  position: relative;
  border-radius: 6px;
}
.mac.window {
  box-shadow: 0 18px 40px 10px rgba(0, 0, 0, 0.36);
}
.mac .title-bar {
  height: 40px;
  background: linear-gradient(to bottom, #f0f0f0, #e9e9e9);
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;
  border-top: 1px solid #faf9ff;
}
.mac .close,
.mac .minimize,
.mac .zoom {
  height: 12px;
  width: 12px;
  border-radius: 6px;
  background: #ddd;
  position: absolute;
  top: 14px;
}
.mac .close {
  background-color: #ff6052;
  left: 10px;
  opacity: 1;
}
.mac .minimize {
  background-color: #ffbe06;
  left: 32px;
}
.mac .zoom {
  background-color: #16cc37;
  left: 54px;
}
</style>
