<template>
  <div>
    <div class="linear-chart">
      <div v-bind:class="placeIndicator" class="indicator"></div>
      <div class="levels">
        <div class="level level-0"></div>
        <div class="level level-1"></div>
        <div class="level level-2"></div>
        <div class="level level-3"></div>
      </div>

      <div class="labels">
        <div class="label label-0">{{minIndicator}}</div>
        <div class="label label-50">{{medIndicator}}</div>
        <div class="label label-100">{{maxIndicator}}</div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "GaugeCompo",
  props: ["nbTweetDay", "nbTweetMax"],
  data() {
    return {
      cardIsExpand: false,
      minIndicator: 0,
      medIndicator: 50,
      maxIndicator: 100
    };
  },
  mounted() {
    this.calculIndicator();
  },

  methods: {
    calculIndicator() {
      this.minIndicator = 0;
      this.maxIndicator = Math.round(this.nbTweetMax);
      this.medIndicator = Math.round(this.nbTweetMax / 2);
    }
  },
  computed: {
    placeIndicator: function() {
      let percentTweet = Math.round((this.nbTweetDay / this.nbTweetMax) * 100);
      let indexIndicator = percentTweet > 99 ? 99 : percentTweet;
      let colorIndicator =
        indexIndicator > 74
          ? "indicator-color-74"
          : indexIndicator > 49
          ? "indicator-color-49"
          : indexIndicator > 24
          ? "indicator-color-25"
          : "indicator-color-0";
      return ["indicator-" + indexIndicator, colorIndicator];
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.linear-chart {
  position: relative;
  padding-top: 14px;
  width: 100%;
}
.linear-chart .indicator {
  width: 0;
  height: 0;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  border-top: 10px solid #000;
  position: absolute;
  margin-left: -5%;
  top: 0;
  left: 0%;
}
.linear-chart .levels {
  font-size: 0;
}
.linear-chart .levels .level {
  content: "";
  display: inline-block;
  width: 24%;
  height: 10px;
  margin: 0 1% 0 0;
}
.linear-chart .levels .level:last-child {
  margin-right: 0 !important;
}
.linear-chart .levels .level.level-0 {
  background-color: #fe1541;
}
.linear-chart .levels .level.level-1 {
  background-color: #fb8b00;
}
.linear-chart .levels .level.level-2 {
  background-color: #05e573;
}
.linear-chart .levels .level.level-3 {
  background-color: #01c950;
}

.linear-chart .labels {
  font-size: 1;
  height: 30px;
}
.linear-chart .labels .label {
  position: absolute;
  left: 0;
  bottom: 0;
}
.linear-chart .labels .label.label-50 {
  left: 50%;
}
.linear-chart .labels .label.label-100 {
  left: auto;
  right: 2px;
}

/*Indicator Arrow*/
/*Color*/
.indicator-color-0 {
  border-top-color: #fe1541 !important;
}
.indicator-color-25 {
  border-top-color: #fb8b00 !important;
}
.indicator-color-50 {
  border-top-color: #05e573 !important;
}
.indicator-color-75 {
  border-top-color: #01c950 !important;
}

/*Position*/
.linear-chart .indicator.indicator-0 {
  left: 0%;
}
.linear-chart .indicator.indicator-1 {
  left: 1%;
}
.linear-chart .indicator.indicator-2 {
  left: 2%;
}
.linear-chart .indicator.indicator-3 {
  left: 3%;
}
.linear-chart .indicator.indicator-4 {
  left: 4%;
}
.linear-chart .indicator.indicator-5 {
  left: 5%;
}
.linear-chart .indicator.indicator-6 {
  left: 6%;
}
.linear-chart .indicator.indicator-7 {
  left: 7%;
}
.linear-chart .indicator.indicator-8 {
  left: 8%;
}
.linear-chart .indicator.indicator-9 {
  left: 9%;
}
.linear-chart .indicator.indicator-10 {
  left: 10%;
}
.linear-chart .indicator.indicator-11 {
  left: 11%;
}
.linear-chart .indicator.indicator-12 {
  left: 12%;
}
.linear-chart .indicator.indicator-13 {
  left: 13%;
}
.linear-chart .indicator.indicator-14 {
  left: 14%;
}
.linear-chart .indicator.indicator-15 {
  left: 15%;
}
.linear-chart .indicator.indicator-16 {
  left: 16%;
}
.linear-chart .indicator.indicator-17 {
  left: 17%;
}
.linear-chart .indicator.indicator-18 {
  left: 18%;
}
.linear-chart .indicator.indicator-19 {
  left: 19%;
}
.linear-chart .indicator.indicator-20 {
  left: 20%;
}
.linear-chart .indicator.indicator-21 {
  left: 21%;
}
.linear-chart .indicator.indicator-22 {
  left: 22%;
}
.linear-chart .indicator.indicator-23 {
  left: 23%;
}
.linear-chart .indicator.indicator-24 {
  left: 24%;
}
.linear-chart .indicator.indicator-25 {
  left: 25%;
}
.linear-chart .indicator.indicator-26 {
  left: 26%;
}
.linear-chart .indicator.indicator-27 {
  left: 27%;
}
.linear-chart .indicator.indicator-28 {
  left: 28%;
}
.linear-chart .indicator.indicator-29 {
  left: 29%;
}
.linear-chart .indicator.indicator-30 {
  left: 30%;
}
.linear-chart .indicator.indicator-31 {
  left: 31%;
}
.linear-chart .indicator.indicator-32 {
  left: 32%;
}
.linear-chart .indicator.indicator-33 {
  left: 33%;
}
.linear-chart .indicator.indicator-34 {
  left: 34%;
}
.linear-chart .indicator.indicator-35 {
  left: 35%;
}
.linear-chart .indicator.indicator-36 {
  left: 36%;
}
.linear-chart .indicator.indicator-37 {
  left: 37%;
}
.linear-chart .indicator.indicator-38 {
  left: 38%;
}
.linear-chart .indicator.indicator-39 {
  left: 39%;
}
.linear-chart .indicator.indicator-40 {
  left: 40%;
}
.linear-chart .indicator.indicator-41 {
  left: 41%;
}
.linear-chart .indicator.indicator-42 {
  left: 42%;
}
.linear-chart .indicator.indicator-43 {
  left: 43%;
}
.linear-chart .indicator.indicator-44 {
  left: 44%;
}
.linear-chart .indicator.indicator-45 {
  left: 45%;
}
.linear-chart .indicator.indicator-46 {
  left: 46%;
}
.linear-chart .indicator.indicator-47 {
  left: 47%;
}
.linear-chart .indicator.indicator-48 {
  left: 48%;
}
.linear-chart .indicator.indicator-49 {
  left: 49%;
}
.linear-chart .indicator.indicator-50 {
  left: 50%;
}
.linear-chart .indicator.indicator-51 {
  left: 51%;
}
.linear-chart .indicator.indicator-52 {
  left: 52%;
}
.linear-chart .indicator.indicator-53 {
  left: 53%;
}
.linear-chart .indicator.indicator-54 {
  left: 54%;
}
.linear-chart .indicator.indicator-55 {
  left: 55%;
}
.linear-chart .indicator.indicator-56 {
  left: 56%;
}
.linear-chart .indicator.indicator-57 {
  left: 57%;
}
.linear-chart .indicator.indicator-58 {
  left: 58%;
}
.linear-chart .indicator.indicator-59 {
  left: 59%;
}
.linear-chart .indicator.indicator-60 {
  left: 60%;
}
.linear-chart .indicator.indicator-61 {
  left: 61%;
}
.linear-chart .indicator.indicator-62 {
  left: 62%;
}
.linear-chart .indicator.indicator-63 {
  left: 63%;
}
.linear-chart .indicator.indicator-64 {
  left: 64%;
}
.linear-chart .indicator.indicator-65 {
  left: 65%;
}
.linear-chart .indicator.indicator-66 {
  left: 66%;
}
.linear-chart .indicator.indicator-67 {
  left: 67%;
}
.linear-chart .indicator.indicator-68 {
  left: 68%;
}
.linear-chart .indicator.indicator-69 {
  left: 69%;
}
.linear-chart .indicator.indicator-70 {
  left: 70%;
}
.linear-chart .indicator.indicator-71 {
  left: 71%;
}
.linear-chart .indicator.indicator-72 {
  left: 72%;
}
.linear-chart .indicator.indicator-73 {
  left: 73%;
}
.linear-chart .indicator.indicator-74 {
  left: 74%;
}
.linear-chart .indicator.indicator-75 {
  left: 75%;
}
.linear-chart .indicator.indicator-76 {
  left: 76%;
}
.linear-chart .indicator.indicator-77 {
  left: 77%;
}
.linear-chart .indicator.indicator-78 {
  left: 78%;
}
.linear-chart .indicator.indicator-79 {
  left: 79%;
}
.linear-chart .indicator.indicator-80 {
  left: 80%;
}
.linear-chart .indicator.indicator-81 {
  left: 81%;
}
.linear-chart .indicator.indicator-82 {
  left: 82%;
}
.linear-chart .indicator.indicator-83 {
  left: 83%;
}
.linear-chart .indicator.indicator-84 {
  left: 84%;
}
.linear-chart .indicator.indicator-85 {
  left: 85%;
}
.linear-chart .indicator.indicator-86 {
  left: 86%;
}
.linear-chart .indicator.indicator-87 {
  left: 87%;
}
.linear-chart .indicator.indicator-88 {
  left: 88%;
}
.linear-chart .indicator.indicator-89 {
  left: 89%;
}
.linear-chart .indicator.indicator-90 {
  left: 90%;
}
.linear-chart .indicator.indicator-91 {
  left: 91%;
}
.linear-chart .indicator.indicator-92 {
  left: 92%;
}
.linear-chart .indicator.indicator-93 {
  left: 93%;
}
.linear-chart .indicator.indicator-94 {
  left: 94%;
}
.linear-chart .indicator.indicator-95 {
  left: 95%;
}
.linear-chart .indicator.indicator-96 {
  left: 96%;
}
.linear-chart .indicator.indicator-97 {
  left: 97%;
}
.linear-chart .indicator.indicator-98 {
  left: 98%;
}
.linear-chart .indicator.indicator-99 {
  left: 99%;
}
</style>
