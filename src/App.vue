<template>
  <div class="container mt-5">
    <vue-p5 @setup="setup"> </vue-p5>
  </div>
</template>
<script>
import VueP5 from "vue-p5";

export default {
  name: "App",
  methods: {
    setup(sketch) {
      sketch.createCanvas(900, 900);
      sketch.background("green");
    }
  },
  components: {
    "vue-p5": VueP5
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
</style>
