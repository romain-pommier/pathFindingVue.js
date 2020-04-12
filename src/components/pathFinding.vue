<template>
  <div class="container mt-5">
    <label for="size">Pixel²</label>
    <input type="number" v-model="size" id="size" />
    <vue-p5 v-on:keyup.enter="setup" @sketch="sketch"></vue-p5>
  </div>
</template>
<script>
import VueP5 from "vue-p5";

export default {
  name: "pathFinding",
  data: function() {
    return {
      p5: null,
      size: null,
      cols: 5,
      rows: 5,
      grid: [this.cols, this.rows],
      openSet: [],
      closedSet: [],
      start: null,
      end: null,
      w: null,
      h: null,
      path: []
    };
  },
  watch: {
    size: function() {
      this.setup(this.p5);
      this.draw(this.p5);
    }
  },
  methods: {
    sketch: function(sk) {
      this.p5 = sk;
    },
    Spot: function() {
      this.f = 0;
      this.g = 0;
      this.h = 0;
    },
    setup: function(sk) {
      console.log(sk);
      sk.createCanvas(this.size, this.size);
      for (let i = 0; i < this.cols; i++) {
        this.grid[i] = new Array(this.rows);
      }
      console.log(this.grid);
    },
    draw(sk) {
      sk.background(0);
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
