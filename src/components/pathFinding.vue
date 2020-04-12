<template>
  <div class="container mt-5">
    <div>
      <input type="number" min="100" max="700" v-model="size" id="size" />
      <label for="size">=> Pixel² (Taille du tableau)</label>
    </div>

    <div>
      <input type="number" min="3" max="60" v-model="cols" id="cols" />
      <label for="cols">=> Nombre de Colonnes</label>
    </div>

    <div>
      <input type="number" min="3" max="60" v-model="rows" id="rows" />
      <label for="rows">=> Nombre de Ligne</label>
    </div>

    <div>
      <input type="number" v-model="indexRandom" id="indexRandom" />
      <label for="indexRandom">=> Pourcentage d'Obstacles</label>
    </div>

    <vue-p5 @sketch="sketch" @setup="setup" @draw="draw" @Spot="Spot"></vue-p5>
  </div>
</template>
<script>
import VueP5 from "vue-p5";

export default {
  name: "pathFinding",
  data: function() {
    return {
      p5: null,
      size: 600,
      cols: 25,
      rows: 25,
      indexRandom: 0.2,
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
  created: function() {},
  watch: {
    // size: function() {
    //   this.setup(this.p5);
    //   this.draw(this.p5);
    // },
    // cols: function() {
    //   this.setup(this.p5);
    //   this.draw(this.p5);
    // },
    // rows: function() {
    //   this.setup(this.p5);
    //   this.draw(this.p5);
    // }
  },
  methods: {
    sketch: function(sk) {
      this.p5 = sk;
    },

    Spot: function(i, j, sk, randomIndex) {
      this.i = i;
      this.j = j;

      this.f = 0;
      this.g = 0;
      this.h = 0;

      this.neighbors = [];
      this.previous = undefined;
      this.wall = false;
      console.log(randomIndex);

      if (sk.random(1) < randomIndex) {
        this.wall = true;
      }

      this.show = function(sk, w, h, col) {
        sk.fill(col);
        if (this.wall) {
          sk.fill(0);
        }
        sk.noStroke(0);
        sk.rect(this.i * w, this.j * h, w - 1, h - 1);
      };
      this.addNeighbors = function(
        grid,
        cols,
        rows,
        currentIndexI,
        currentIndexJ
      ) {
        console.log(this);
        let i = currentIndexI;
        let j = currentIndexJ;

        if (i < cols - 1) {
          this.neighbors.push(grid[i + 1][j]);
        }
        if (i > 0) {
          this.neighbors.push(grid[i - 1][j]);
        }
        if (j < rows - 1) {
          this.neighbors.push(grid[i][j + 1]);
        }
        if (j > 0) {
          this.neighbors.push(grid[i][j - 1]);
        }
        if (i > 0 && j > 0) {
          this.neighbors.push(grid[i - 1][j - 1]);
        }
        if (i < cols - 1 && j > 0) {
          this.neighbors.push(grid[i + 1][j - 1]);
        }
        if (i > 0 && j < rows - 1) {
          this.neighbors.push(grid[i - 1][j + 1]);
        }
        if (i < cols - 1 && j < rows - 1) {
          this.neighbors.push(grid[i + 1][j + 1]);
        }
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
          this.grid[i][j] = new this.Spot(i, j, sk, this.indexRandom);
        }
      }

      for (let i = 0; i < this.cols; i++) {
        for (let j = 0; j < this.rows; j++) {
          let currentIndexI = i;
          let currentIndexJ = j;
          this.grid[i][j].addNeighbors(
            this.grid,
            this.cols,
            this.rows,
            currentIndexI,
            currentIndexJ
          );
        }
      }

      this.start = this.grid[0][0];
      this.end = this.grid[this.cols - 1][this.rows - 1];
      this.start.wall = false;
      this.end.wall = false;

      this.openSet.push(this.start);
    },

    draw(sk) {
      if (this.openSet.length > 0) {
        let winner = 0;
        for (let i = 0; i < this.openSet.length; i++) {
          if (this.openSet[i].f < this.openSet[winner].f) {
            winner = i;
          }
        }

        var current = this.openSet[winner];

        if (this.openSet[winner] === this.end) {
          sk.noLoop();
          console.log("End");
        }

        this.removeFromArray(this.openSet, current);
        this.closedSet.push(current);

        let neighbors = current.neighbors;
        for (let i = 0; i < neighbors.length; i++) {
          let neighbor = neighbors[i];

          if (!this.closedSet.includes(neighbor) && !neighbor.wall) {
            let tempG = current.g + 1;
            var newPath = false;

            if (this.openSet.includes(neighbor)) {
              if (tempG < neighbor.g) {
                neighbor.g = tempG;
                newPath = true;
              }
            } else {
              neighbor.g = tempG;
              newPath = true;
              this.openSet.push(neighbor);
            }
            if (newPath) {
              neighbor.h = this.heuristic(neighbor, this.end, sk);
              neighbor.f = neighbor.g + neighbor.h;
              neighbor.previous = current;
            }
          }
        }
      } else {
        console.log("no solution");
        sk.noLoop();
        return;
      }
      sk.background(0);

      for (let i = 0; i < this.cols; i++) {
        for (let j = 0; j < this.rows; j++) {
          this.grid[i][j].show(sk, this.w, this.h, sk.color(255));
        }
      }

      for (let i = 0; i < this.closedSet.length; i++) {
        this.closedSet[i].show(sk, this.w, this.h, sk.color(255, 0, 0));
      }
      for (let i = 0; i < this.openSet.length; i++) {
        this.openSet[i].show(sk, this.w, this.h, sk.color(0, 255, 0));
      }
      this.path = [];
      let temp = current;
      this.path.push(temp);

      while (temp.previous) {
        this.path.push(temp.previous);
        temp = temp.previous;
      }

      for (let i = 0; i < this.path.length; i++) {
        this.path[i].show(sk, this.w, this.h, sk.color(0, 0, 255));
      }
    },
    removeFromArray(arr, elt) {
      for (let i = arr.length - 1; i >= 0; i--) {
        if (arr[i] == elt) {
          arr.splice(i, 1);
        }
      }
    },
    heuristic(a, b, sk) {
      let d = sk.dist(a.i, a.j, b.i, b.j);
      // let d = abs(a.i - b.i) + abs(a.j - b.j);
      return d;
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
