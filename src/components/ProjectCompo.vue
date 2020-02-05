<template >
  <div>
    <h1 class="titlePage" v-scroll-reveal>Polytweet</h1>

    <div class="counter">
      <!-- Affichage des Tweets -->
      <div v-if="showTweet" class="full-height-width">
        <div v-for="(item) in tweets" v-bind:key="item._id">
          <div v-bind:style="item.placement" class="tweet" v-scroll-reveal="{ duration: 1250 }">
            <h6>#{{ item.hashtag }}</h6>
            <span class="text-tweet">{{item.text}}</span>
          </div>
        </div>
      </div>
      <!--/ Affichage des Tweets -->
      <!-- Animation -->
      <div class="d-flex flex-column counterAnim">
        <h1 v-scroll-reveal class="title-top">Déjà plus de</h1>
        <animated-number
          class="my-0"
          :value="value"
          :formatValue="formatToPrice"
          :duration="1000"
          :round="1"
          v-scroll-reveal
        />
        <h1 v-scroll-reveal class="title-bottom">tweets analysés</h1>
      </div>
      <!--/ Animation -->
    </div>

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
import AnimatedNumber from "animated-number-vue";
import gql from "graphql-tag";
export default {
  name: "ProjectCompo",
  components: {
    AnimatedNumber
  },
  data() {
    return {
      value: 0,
      showCounter: false,
      showTweet: false,
      tweets: [],
      placementAllReadyTake: [],
      placementTweetIndexAvaible: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11],
      placement: [
        { top: "10%", right: null, bottom: null, left: "15%" },
        { top: "10%", right: null, bottom: null, left: "35%" },
        { top: "10%", right: null, bottom: null, left: "55%" },
        { top: "10%", right: null, bottom: null, left: "75%" },
        { top: "35%", right: "10%", bottom: null, left: null },
        { top: "60%", right: "10%", bottom: null, left: null },
        { top: null, right: null, bottom: "10%", left: "15%" },
        { top: null, right: null, bottom: "10%", left: "35%" },
        { top: null, right: null, bottom: "10%", left: "55%" },
        { top: null, right: null, bottom: "10%", left: "75%" },
        { top: "35%", right: null, bottom: null, left: "10%" },
        { top: "60%", right: null, bottom: null, left: "10%" }
      ]
    };
  },
  props: {},
  created() {
    // Query for number of tweets
    this.$apollo.addSmartQuery("nbTweets", {
      query: gql`
        query {
          totalNumberOfTweetsUsedByPolytweet
        }
      `,
      update(data) {
        this.value = data.totalNumberOfTweetsUsedByPolytweet;
        // The returned value will update
        return data.totalNumberOfTweetsUsedByPolytweet;
      },
      pollInterval: 1000 // ms
    });

    // Query for number of tweets
    this.$apollo.addSmartQuery("last10Tweets", {
      query: gql`
        query {
          last10tweets {
            _id
            hashtag
            text
          }
        }
      `,
      update(data) {
        //Cherche les éléments différents entre l'ancienne et la nouvelle requête
        const elemDiff = data.last10tweets.filter(
          elem => !this.tweets.find(({ _id }) => elem._id === _id)
        );
        //Pour chaque élément différent ajoute les données et remove ancien tweet
        elemDiff.forEach(e => {
          e.placement = this.tweets[this.tweets.length - 1].placement;
          e.hashtag = Array.isArray(e.hashtag) ? e.hashtag[0] : e.hashtag;
          this.tweets.pop();
          this.tweets.unshift(e);
        });
        return data;
      },
      pollInterval: 1000 // ms
    });

    this.getLast10Tweet();

    setInterval(() => {
      this.$apollo.queries.last10Tweets.refresh();
      this.$apollo.queries.nbTweets.refresh();
    }, 10000);
  },
  methods: {
    formatToPrice(value) {
      return `${value}`;
    },
    placeTweetOnScreen() {
      for (let i = 0; i < this.tweets.length; i++) {
        let rand = Math.floor(
          Math.random() * Math.floor(this.placementTweetIndexAvaible.length)
        );
        let indexPlacement = this.placementTweetIndexAvaible[rand];
        this.placementTweetIndexAvaible.splice(
          this.placementTweetIndexAvaible.indexOf(indexPlacement),
          1
        );
        this.tweets[i].placement = this.placement[indexPlacement];
        this.placementAllReadyTake.push(indexPlacement);
      }
      this.showTweet = true;
    },
    async getLast10Tweet() {
      let resApollo = await this.$apollo.query({
        query: gql`
          query {
            last10tweets {
              _id
              hashtag
              text
            }
          }
        `
      });
      let tweetBeforeTransform = resApollo.data.last10tweets;
      tweetBeforeTransform.forEach(element => {
        element.hashtag = Array.isArray(element.hashtag)
          ? element.hashtag[0]
          : element.hashtag;
        element.placement = "";
        element.isNew = false;
      });
      this.tweets = tweetBeforeTransform;
      this.placeTweetOnScreen();
    }
  },
  watch: {}
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

/* =================
     COMPTEUR
 ================= */
.counter {
  margin-top: -60px;
  min-height: 80vh;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.counterAnim span {
  font-family: "Airbnb Cereal App Bold";
  font-size: 150px;
  font-weight: 800;
}
.full-height-width {
  position: absolute;
  height: 80vh;
  width: 100vw;
}
.title-top {
  margin-bottom: -40px;
}
.title-bottom {
  margin-top: -40px;
}

/*Tweets placement*/
.tweet {
  position: absolute;
  max-width: 250px;
  overflow-wrap: break-word;
}
span.text-tweet {
  display: block;
  width: 250px;
  line-height: 1;
  font-size: 13px;
}

/* change navbar background on collapse */
@media (max-width: 1300px) {
  .counterAnim span {
    font-size: 100px;
  }
}
</style>
