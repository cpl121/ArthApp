<template>
  <div id="app">
    <h1>ARTH-APP</h1>
    <b-button id="playButton" variant="outline-success" @click="play=!play" v-if="!play"><b-icon-play-fill animation="throb" scale="3"></b-icon-play-fill></b-button>
    <div v-if="play">
      <Home />
    </div>
    <b-button variant="secondary" @click="play=!play" v-if="play"><b-icon-arrow-return-left scale="1,5"></b-icon-arrow-return-left></b-button>
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
  color: #2c3e50;
  margin-top: 60px;
}

#playButton {
  margin-top: 10%;
  width: 100px;
  height: 60px;
}

</style>
