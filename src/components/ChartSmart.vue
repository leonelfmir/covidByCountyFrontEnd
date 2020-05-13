<template>
  <div>
    <div v-if="csvData !== null">
      <ChartC :chart-data="csv" class="cnv" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ChartC from "./ChartC.vue";
const apiUrl = "http://localhost:3030/csv/county/";

export default {
  name: "ChartSmart",
  components: {
    ChartC
  },
  props: {
    county: String
  },
  data() {
    return {
      csvData: null
    };
  },
  mounted() {
    const url = `${apiUrl}${c}`;
    axios.get(url).then(response => (this.csvData = response.data));
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
