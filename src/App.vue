<template>
  <div id="app">
    <h1 v-if="play">ARTH-APP</h1>
    <br>
    <img src="./assets/ArthApp-min.jpg" alt="Logo" width="180" height="180" v-if="!play">
    <br>
    <b-button pill id="playButton" variant="outline-success" @click="play=!play" v-if="!play"><b-icon-play-fill animation="throb" scale="3"></b-icon-play-fill></b-button>
    <div v-if="play">
      <Home />
    </div>
    <b-button variant="secondary" @click="play=!play" v-if="play"><b-icon-arrow-return-left scale="1,5"></b-icon-arrow-return-left></b-button>
    <br>
    <br>
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
      play: false,
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  align-items: center;
  color: #2c3e50;
  margin-top: 60px;
}

#playButton {
  margin-top: 10%;
  width: 100px;
  height: 60px;
}

</style>
