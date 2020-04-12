<template>
  <div class="container mt-5">
    <label for="size">Pixel²</label>
    <input type="number" v-model="dataPathFinding.size" id="size" />
    <label for="cols">Cols</label>
    <input type="number" v-model="cols" id="cols" />
    <vue-p5 @sketch="sketch"></vue-p5>
  </div>
</template>
<script>
import VueP5 from "vue-p5";

export default {
  name: "pathFinding",
  data: function() {
    return {
      p5: null,
      dataPathFinding: {
        size: null,
        cols: 5,
        rows: 5
      },
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
    "dataPathFinding.size": function() {
      console.log("here");
      this.setup(this.p5);
      this.draw(this.p5);
    }
  },
  methods: {
    sketch: function(sk) {
      this.p5 = sk;
    },

    Spot: function(i, j) {
      this.x = i;
      this.y = j;

      this.f = 0;
      this.g = 0;
      this.h = 0;

      this.show = function(sk, w, h, col) {
        sk.fill(col);
        sk.noStroke(0);
        sk.rect(this.x * w, this.y * h, w - 1, h - 1);
      };
    },

    setup: function(sk) {
      sk.createCanvas(this.size, this.size);

      this.w = sk.width / this.cols;
      this.h = sk.height / this.rows;

      for (let i = 0; i < this.cols; i++) {
        this.grid[i] = new Array(this.rows);
      }

      for (let i = 0; i < this.cols; i++) {
        for (let j = 0; j < this.rows; j++) {
          this.grid[i][j] = new this.Spot(i, j);
        }
      }

      this.start = this.grid[0][0];
      this.end = this.grid[this.cols - 1][this.rows - 1];

      this.openSet.push(this.start);
    },

    draw(sk) {
      if (this.openSet.length > 0) {
      } else {
      }
      sk.background(0);

      for (let i = 0; i < this.cols; i++) {
        for (let j = 0; j < this.cols; j++) {
          this.grid[i][j].show(sk, this.w, this.h, sk.color(255));
        }
      }

      for (let i = 0; i < this.closedSet.length; i++) {
        this.closedSet[i].show(sk, this.w, this.h, sk.color(255, 0, 0));
      }
      for (let i = 0; i < this.openSet.length; i++) {
        this.openSet[i].show(sk, this.w, this.h, sk.color(0, 255, 0));
      }
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
