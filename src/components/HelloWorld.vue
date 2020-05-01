<template>
  <div class="cnv">
    <canvas ref="myChart" width="400" height="400"></canvas>
  </div>
</template>

<script>
import axios from "axios";
import Chart from "chart.js";

const apiUrl = "http://localhost:3030/csv/Miami-Dade";

function createChart(ctx, data) {
  const labels = data.map(d => d.Updated);
  const confirmed = data.map(d => d.Confirmed);
  let incr = [0];
  for (let i = 1; i < confirmed.length; i++) {
    incr.push(confirmed[i] - confirmed[i - 1]);
  }

  const deaths = data.map(d => d.Deaths);
  let deathsChange = [0];
  deaths.reduce((prev, curr) => {
    deathsChange.push(curr - prev);
    return curr;
  });

  const datasets = [
    {
      label: "Confirmed",
      data: confirmed,
      borderWidth: 1,
      order: 2,
      backgroundColor: "rgb(204, 255, 153)"
    },
    {
      label: "Increment",
      data: incr,
      borderWidth: 1,
      order: 1,
      backgroundColor: "rgb(204, 204, 255)"
    },
    {
      label: "Death",
      data: deaths,
      borderWidth: 1,
      order: 1,
      backgroundColor: "rgb(204, 20, 255)"
    },
    {
      label: "Death Change",
      data: deathsChange,
      borderWidth: 1,
      order: 1,
      backgroundColor: "rgb(24, 204, 255)"
    }
  ];
  paintChart(ctx, labels, datasets);
}

function paintChart(ctx, labels, datasets) {
  new Chart(ctx, {
    type: "bar",
    data: {
      labels,
      datasets
    },
    options: {
      maintainAspectRatio: false,
      scales: {
        yAxes: [
          {
            ticks: {
              beginAtZero: true
            }
          }
        ]
      }
    }
  });
}

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      csvData: Object
    };
  },
  methods: {
    printChart(data) {
      const ctx = this.$refs.myChart;
      createChart(ctx, data);
    }
  },
  mounted() {
    axios
      .get(apiUrl)
      .then(response => response.data)
      // .then(dt => neatCsv(dt))
      .then(dt => {
        this.csvData = dt;
        this.printChart(dt);
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cnv {
  width: 1500px;
  height: 800px;
}
</style>
