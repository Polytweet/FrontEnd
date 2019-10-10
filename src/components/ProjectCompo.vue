<template >
  <div class="container">
    <h1>PolyTweet</h1>
     <div>{{communes}}</div>
  </div>
</template>


 

<script>
import gql from 'graphql-tag';
export default {
  name: "ProjectCompo",
  mounted() {},
  created() {
  
  },
  methods: {},
 apollo: {
    // Simple query that will update the 'hello' vue property
    communes: gql`query {
 communes(code_dept: "69") {
    type
    property: fields{
      population
      superficie
    }
    geometry{
      type: _type
      coordinates
    }
	}
}`,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
</style>
