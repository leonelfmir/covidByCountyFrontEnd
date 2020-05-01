<template>
  <div>
    <div v-if="csvData !== null">
      <div v-for="csv in csvData" :key="csv" >
        <ChartC :chart-data="csv" class="cnv"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ChartC from "./ChartC.vue";
const apiUrl = "http://localhost:3030/csv/"; //Miami-Dade";

export default {
  name: "Charts",
  components: {
    ChartC
  },
  data() {
    return {
      csvData: null,
      countys: ['Miami-Dade', 'Broward']
    };
  },
  mounted() {
    const proms = this.countys.map(c => {
      const url = `${apiUrl}${c}`;
      return axios.get(url).then(response => response.data);
    });
    Promise.all(proms).then(v => (this.csvData = v));
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cnv {
  width: 400;
  height: 400px;
}
</style>
