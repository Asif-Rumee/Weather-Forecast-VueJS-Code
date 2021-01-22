<template>
  <div>
    <canvas></canvas>
  </div>
</template>
<script>
import Chart from "chart.js";
export default {
  props: {
    chartType: {
      type: String,
      required: true
    },
    chartData: {
      type: Object,
      required: true
    },
    chartOptions: {
      type: Object,
      required: true
    }
  },

  methods: {
    chartConstructor(chartType, chartData, chartOptions) {
      const chartElement = document.querySelector("canvas");
      const chart = new Chart(chartElement, {
        type: chartType,
        data: chartData,
        options: chartOptions
      });
      return chart;
    }
  },
  mounted() {
    let { chartType, chartData, chartOptions } = this;
    this.chartConstructor(chartType, chartData, chartOptions);
  }
};
</script>
<style lang="scss" scoped>
canvas {
  height: 250px;
}
</style>
