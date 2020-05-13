<template>
  <div>
    <div v-for="county in counties" :key="county">
      <ChartSmart :county="county" class="cnv" />
    </div>
  </div>
</template>

<script>
import ChartSmart from "./ChartSmart.vue";

export default {
  name: "Charts",
  components: {
    ChartSmart
  },
  props: {
    counties: Array
  },
  data() {
    return {};
  },
  mounted() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cnv {
  width: 400;
  height: 400px;
}
</style>
