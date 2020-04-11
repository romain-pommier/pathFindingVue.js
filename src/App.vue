<template>
  <div class="container mt-5">
    <input type="number" name v-model="height" id />
    <vue-p5 @setup="setup"></vue-p5>
  </div>
</template>
<script>
import VueP5 from "vue-p5";

export default {
  name: "App",
  data: function() {
    return {
      height: null
    };
  },
  methods: {
    setup(sketch) {
      sketch.createCanvas(parseInt(900), 900);
      sketch.background("green");
    }
  },
  updated: function() {},
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
