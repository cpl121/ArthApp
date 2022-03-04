<template>
  <div id="app">
    <h1 v-if="play">ARTH-APP</h1>
    <div class="homePage" v-if="!play">
      <img
        src="./assets/ArthApp-min.jpg"
        alt="Logo"
        width="180"
        height="180"
      />
      <hr>
      <b-button
        pill
        id="playButton"
        variant="outline-success"
        @click="play = !play"
        ><b-icon-play-fill animation="throb" scale="3"></b-icon-play-fill
      ></b-button>
    </div>
    <div v-if="play">
      <Home />
    </div>
    <b-button variant="secondary" @click="play = !play" v-if="play"
      ><b-icon-arrow-return-left scale="1,5"></b-icon-arrow-return-left
    ></b-button>
    <br />
    <div class="ads"></div>
  </div>
</template>

<script>
import Home from "./components/Home.vue";

export default {
  name: "App",
  components: {
    Home,
  },
  data() {
    return {
      play: false
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding-bottom: 5%;
}

#playButton {
  margin-top: 10%;
  width: 100px;
  height: 60px;
}

.homePage {
  margin: 1%;
  padding: 5%;
  display: inline-block;
}

.ads {
  padding: 10% ;
}

</style>
