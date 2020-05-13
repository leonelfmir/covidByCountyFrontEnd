<template>
  <div>
    <div v-if="counties !== null">
      <multiselect
        v-model="selectedCounties"
        :options="counties"
        :multiple="true"
        @input="selected"
      ></multiselect>
    </div>
  </div>
</template>

<script>
import axios from "axios";
const apiUrl = "http://localhost:3030/csv/counties";
const startingCounties = ["Miami-Dade County", "Broward County"];

export default {
  name: "CountySelection",
  data() {
    return {
      counties: [1, 2],
      selectedCounties: startingCounties
    };
  },
  mounted() {
    axios.get(apiUrl).then(response => {
      const counties = response.data;
      counties.sort();
      this.counties = counties;
      this.selected(this.selectedCounties);
    });
  },
  methods: {
    selected(selection) {
      this.$emit("selection", selection);
    }
  }
};
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cnv {
  width: 400;
  height: 400px;
}
</style>
