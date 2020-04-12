<template>
  <div class="container mt-5">
    <pathFinding></pathFinding>
  </div>
</template>
<script>
import pathFinding from "./components/pathFinding.vue";

export default {
  name: "App",
  data: function() {
    return {};
  },
  methods: {},
  updated: function() {},
  components: {
    pathFinding: pathFinding
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
