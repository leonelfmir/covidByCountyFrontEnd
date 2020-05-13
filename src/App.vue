<template>
  <div id="app">
    <CountySelection @selection="CountySelection" />
    <div v-if="anyCountieSelecte">
      <Charts :counties="countiesSelected" />
    </div>
  </div>
</template>

<script>
import Charts from "./components/Charts.vue";
import CountySelection from "./components/CountySelection.vue";

export default {
  name: "App",
  components: {
    Charts,
    CountySelection
  },
  data() {
    return {
      countiesSelected: []
    };
  },
  methods: {
    CountySelection(selection) {
      this.countiesSelected = selection;
    }
  },
  computed: {
    anyCountieSelecte() {
      return this.countiesSelected.length > 0;
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
</style>
