<template>
  <div>
    <canvas ref="myChart" width="400" height="400"></canvas>
  </div>
</template>

<script>
import Chart from "chart.js";

function createChart(ctx, data, title) {
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
  paintChart(ctx, labels, datasets, title);
}

function paintChart(ctx, labels, datasets, title) {
  new Chart(ctx, {
    type: "bar",
    data: {
      labels,
      datasets
    },
    options: {
      title: {
        display: true,
        text: title
      },
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
  name: "ChartC",
  props: {
    chartData: Array,
    title: String
  },
  methods: {
    printChart(data, title) {
      const ctx = this.$refs.myChart;
      createChart(ctx, data, title);
    }
  },
  mounted() {
    this.printChart(this.chartData, this.title);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cnv {
  width: 1500px;
  height: 300px;
}
</style>
