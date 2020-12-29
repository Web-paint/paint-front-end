<template>
  <!--<img alt="Vue logo" src="./assets/logo.png" />
  <Bars/>-->
  <div id="app">
    <Canvas />
  </div>
</template>

<script>
//import Bars from "./components/Bars.vue";
import Canvas from "./components/Canvas.vue";

export default {
  name: "App",
  components: {
    //Bars,
    Canvas
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: gray;
  //margin-top: 60px;
}
</style>
