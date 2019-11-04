<template>
  <div>
    <!-- Nav -->
    <nav class="navbar navbar-expand-lg navbar-light">
      <a class="navbar-brand" v-on:click="currentNavLink = 'home'">
        <img alt="Vue logo" src="../assets/logo.png" width="60" />
      </a>
      <button
        class="navbar-toggler"
        type="button"
        data-target="#navbarText"
        aria-controls="navbarText"
        aria-expanded="false"
        aria-label="Toggle navigation"
        v-on:click="toggleNavbar()"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarText" v-bind:class="{ 'show': show }">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item">
            <a class="nav-link" href="carte" :class="[currentNavLink!='home'?'dark':'']">Carte</a>
          </li>
          <li class="nav-item">
            <a
              class="nav-link"
              v-on:click="[currentNavLink = 'project',toggleNavbar()]"
              :class="[currentNavLink=='project'?'active':'',currentNavLink!='home'?'dark':'']"
            >Polytweet</a>
          </li>
          <li class="nav-item">
            <a
              class="nav-link"
              v-on:click="[currentNavLink = 'team',toggleNavbar()]"
              :class="[currentNavLink=='team'?'active':'',currentNavLink!='home'?'dark':'']"
            >L'équipe</a>
          </li>
          <li class="nav-item">
            <a
              class="nav-link"
              v-on:click="[currentNavLink = 'ml',toggleNavbar()]"
              :class="[currentNavLink=='ml'?'active':'',currentNavLink!='home'?'dark':'']"
            >Mentions légales</a>
          </li>
        </ul>
      </div>
    </nav>
    <!-- Home -->
    <div class="home" v-if="currentNavLink=='home'">
      <HomeCompo />
    </div>
    <!-- Project -->
    <div v-else-if="currentNavLink=='project'">
      <ProjectCompo />
    </div>

    <!-- Team -->
    <div v-else-if="currentNavLink=='team'" class="team">
      <TeamCompo />
    </div>

    <!-- Mentions légales -->
    <div v-else-if="currentNavLink=='ml'">
      <MentionCompo />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HomeCompo from "@/components/HomeCompo.vue";
import TeamCompo from "@/components/TeamCompo.vue";
import MentionCompo from "@/components/MentionCompo.vue";
import ProjectCompo from "@/components/ProjectCompo.vue";

export default {
  name: "home",
  data() {
    return {
      currentNavLink: "home",
      show: false
    };
  },
  components: {
    HomeCompo,
    TeamCompo,
    MentionCompo,
    ProjectCompo
  },
  methods: {
    /*Open/Close the nav when click on burger or nav-link */
    toggleNavbar() {
      this.show = !this.show;
    }
  }
};
</script>

<style scoped>
nav {
  background-color: transparent;
  width: 100%;
  font-family: "Airbnb Cereal App Bold";
  position: absolute;
  z-index: 999;
}

/* Brand */
.navbar-brand {
  margin-left: 60px;
}

/* Link */
nav ul li a {
  margin: 0px 20px;
}

nav ul a:hover {
  color: #fff !important;
}

nav ul a::after {
  content: "";
  display: block;
  width: 0;
  height: 2px;
  background: #fff;
  transition: width 0.5s;
}
nav ul a:hover::after {
  width: 100%;
  transition: width 0.5s;
}
.nav ul a.active,
nav ul a.dark:hover {
  color: #333 !important;
}
nav ul a.active::after,
nav ul a.dark::after {
  background: #333 !important;
}
nav ul a.active::after {
  width: 100%;
}
.navbar-toggler {
  border: none;
}
/* change navbar background on collapse */
@media (max-width: 768px) {
  nav.navbar {
    background: #fff;
  }
  .navbar-brand {
    margin-left: 0px;
  }
  nav ul a::after {
    margin-left: auto;
    margin-right: auto;
  }
  nav ul a:hover::after {
    width: 15%;
    margin-left: auto;
    margin-right: auto;
    transition: width 0.5s;
  }
  nav ul a.active::after {
    width: 15%;
  }
}
</style>
