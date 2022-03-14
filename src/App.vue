<template>
  <div id="app">
    <h1 v-if="level !== 0">ARTH-APP</h1>
    <div class="homePage" v-if="level === 0">
      <img src="./assets/ArthApp-min.jpg" alt="Logo" title="App Logo" />
      <hr />

      <div class="levels">
        <b-button
          pill
          id="playButton"
          variant="outline-success"
          @click="level = 1"
          ><b-icon-reception1 scale="2"></b-icon-reception1
        ></b-button>
        <b-button pill id="playButton" variant="outline-info" @click="level = 2"
          ><b-icon-reception2 scale="2"></b-icon-reception2
        ></b-button>
        <b-button
          pill
          id="playButton"
          variant="outline-danger"
          @click="level = 3"
          ><b-icon-reception3 scale="2"></b-icon-reception3
        ></b-button>
        <b-button pill id="playButton" variant="outline-dark" @click="level = 4"
          ><b-icon-reception4 scale="2"></b-icon-reception4
        ></b-button>
      </div>
    </div>
    <div v-if="level !== 0">
      <div>
        <Home :level="level" />
      </div>
      <b-button variant="secondary" @click="level = 0"
        ><b-icon-arrow-return-left scale="1,5"></b-icon-arrow-return-left
      ></b-button>
      <br />
    </div>
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
      level: 0,
    };
  },
};
</script>

<style>
#app {
  font-family: 'Courier New', Courier, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding-bottom: 5%;
}

#playButton {
  position: flex;
  margin: 10%;
  width: 80px;
  height: 50px;
}

.homePage {
  padding: 1%;
  display: inline-block;
}

.levels {
  padding: 5%;
  position: flex;
}

img {
  width: 150px;
  height: 150px;
}
</style>
