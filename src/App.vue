

<template>
  <div id="app">
    <!-- 
    <div id="nav">
  
      <router-link to="/">Home</router-link>|
      <router-link to="/about">About</router-link>
    </div>-->
    <router-view />
  </div>
</template>




<style>
/*Font*/
@font-face {
  font-family: "Airbnb Cereal App Book";
  font-style: normal;
  font-weight: normal;
  src: local("assets/fonts/Airbnb Cereal App Book"),
    url("assets/fonts/AirbnbCerealBook.woff") format("woff");
}

@font-face {
  font-family: "Airbnb Cereal App Light";
  font-style: normal;
  font-weight: normal;
  src: local("assets/fonts/Airbnb Cereal App Light"),
    url("assets/fonts/AirbnbCerealLight.woff") format("woff");
}

@font-face {
  font-family: "Airbnb Cereal App Medium";
  font-style: normal;
  font-weight: normal;
  src: local("assets/fonts/Airbnb Cereal App Medium"),
    url("assets/fonts/AirbnbCerealMedium.woff") format("woff");
}

@font-face {
  font-family: "Airbnb Cereal App Bold";
  font-style: normal;
  font-weight: normal;
  src: local("assets/fonts/Airbnb Cereal App Bold"),
    url("assets/fonts/AirbnbCerealBold.woff") format("woff");
}

@font-face {
  font-family: "Airbnb Cereal App Extra Bold";
  font-style: normal;
  font-weight: normal;
  src: local("assets/fonts/Airbnb Cereal App Extra Bold"),
    url("assets/fonts/AirbnbCerealExtraBold.woff") format("woff");
}

@font-face {
  font-family: "Airbnb Cereal App Black";
  font-style: normal;
  font-weight: normal;
  src: local("assets/fonts/Airbnb Cereal App Black"),
    url("assets/fonts/AirbnbCerealBlack.woff") format("woff");
}

#app {
  font-family: "Airbnb Cereal App Book";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: "Airbnb Cereal App Bold";
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

textarea:hover,
input:hover,
textarea:active,
input:active,
textarea:focus,
input:focus,
button:focus,
button:active,
button:hover,
label:focus,
.btn:active,
.btn.active {
  outline: 0px !important;
  -webkit-appearance: none;
  box-shadow: none !important;
}

a {
  cursor: pointer;
}

/*Btn*/
.btn-primary {
  box-shadow: 0 3px 5px -1px rgba(0, 0, 0, 0.2),
    0 6px 10px 0 rgba(0, 0, 0, 0.14), 0 1px 18px 0 rgba(0, 0, 0, 0.12) !important;
  color: #fff !important;
  background-color: #448cef !important;
  font-size: 1rem !important;
  line-height: 1.2 !important;
  padding: 0.875rem 1.25rem !important;
  border: none !important;
  border-radius: 0.625rem !important;
  cursor: pointer;
  font-family: "Airbnb Cereal App Bold" !important;
}

/*Leaflet*/
@media (max-width: 768px) {
  .leaflet-control-zoom {
    visibility: hidden;
  }
}
</style>
