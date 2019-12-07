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

    <div class="counter" @mouseenter="showCounter=true">
      <!-- Affichage des Tweets -->
      <div v-if="showTweet" class="full-height-width">
        <div v-for="(item, i) in tweetTest" v-bind:key="i+item.hastag">
          <div v-bind:style="item.placement" class="tweet" v-scroll-reveal>
            <h6>{{ item.hastag + i }}</h6>
            <span>{{item.text}}</span>
          </div>
        </div>
      </div>
      <!--/ Affichage des Tweets -->
      <!-- Animation -->
      <div class="d-flex flex-column counterAnim">
        <h1 v-scroll-reveal>Déjà plus de</h1>
        <animated-number
          class="my-5"
          v-if="showCounter"
          :value="value"
          :formatValue="formatToPrice"
          :duration="1000"
          :round="1"
          v-scroll-reveal
        />
        <h1 v-scroll-reveal>tweets analysés</h1>
      </div>
      <!--/ Animation -->
    </div>
  </div>
  <!-- 
  explication projet
  mention legales
  -->
</template>

<script>
import AnimatedNumber from "animated-number-vue";

export default {
  name: "HomeCompo",
  components: {
    AnimatedNumber
  },
  data() {
    return {
      value: 837739,
      showCounter: false,
      showTweet: false,
      tweetTest: [
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        },
        {
          hastag: "#PolyTweet",
          text: "attribute to limit CSS to this ",
          placement: ""
        }
      ]
    };
  },
  props: {},

  methods: {
    formatToPrice(value) {
      return `${value}`;
    },
    placeTweetOnScreen() {
      let style = {
        top: 10 + "%",
        right: -5 + "%",
        bottom: 0 + "%",
        left: -5 + "%"
      };
      for (let i = 0; i < this.tweetTest.length; i++) {
        this.tweetTest[i].placement = this.cssIncrementPosition(style, i);
        let res = i % 12;

        console.log(
          res +
            " - " +
            i +
            "   " +
            "top = " +
            this.tweetTest[i].placement.top +
            " right = " +
            this.tweetTest[i].placement.right +
            "bottom = " +
            this.tweetTest[i].placement.bottom +
            " left = " +
            this.tweetTest[i].placement.left
        );
      }
      this.showTweet = true;
    },
    cssIncrementPosition(style, i) {
      //Version modulo
      let mod = i % 6;
      console.log(mod);
      //Traitement Horizontal
      if (mod <= 3) {
        //Partie du haut
        if (i % 12 < 6) {
          style.right = "0%";
        } else {
          //Partie du bas
          if (i % 12 == 6) {
            style.left = "-5%";
            style.right = "0%";
          }
          style.top = "0%";
          style.bottom = "10%";
        }
        style.left.replace("%", "");
        style.left = parseInt(style.left) + 20 + "%";
      }
      //Traitement Vertical
      else {
        //Partie de droite
        if (i % 12 < 6) {
          if (i % 12 == 4) {
            style.top = "10%";
            style.left = "0%";
            style.right = "10%";
          }
          style.top.replace("%", "");
          style.top = parseInt(style.top) + 25 + "%";
        } else {
          //Partie de gauche
          if (i % 12 == 10) {
            style.top = "10%";
            style.left = "10%";
            style.bottom = "0%";
          }
          style.top.replace("%", "");
          style.top = parseInt(style.top) + 25 + "%";
        }
      }

      let newStyle = {
        top: style.top == "0%" ? null : style.top,
        right: style.right == "0%" ? null : style.right,
        bottom: style.bottom == "0%" ? null : style.bottom,
        left: style.left == "0%" ? null : style.left
      };
      return newStyle;
    }
  },
  watch: {
    showCounter: {
      handler: function(val, oldVal) {
        console.log(val + " " + oldVal);
        if (val && !oldVal) {
          this.placeTweetOnScreen();
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Home */
.home,
.counter,
.full-height-width {
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

/* Compteur */
.counter {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.counterAnim span {
  font-family: "Airbnb Cereal App Bold";
  font-size: 200px;
  font-weight: 800;
}
.full-height-width {
  position: absolute;
}
/*Tweets placement*/
.tweet {
  position: absolute;
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
