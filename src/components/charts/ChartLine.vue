<template>
  <div>
    <chart-base
      :chartType="chartType"
      :chartData="chartData"
      :chartOptions="chartOptions"
    />
  </div>
</template>
<script>
import ChartBase from "../ChartBase.vue";
export default {
  components: {
    ChartBase
  },
  props: {
    chartData: {
      type: Object,
      required: false
    }
  },
  data() {
    return {
      chartType: "line",

      chartOptions: {
        scales: {
          xAxes: [
            {
              stacked: true,
              gridLines: {
                display: false
              }
            }
          ],
          yAxes: [
            {
              gridLines: {
                display: false,
                color: "#ffffff"
              },
              ticks: {
                display: false
                // stepSize: 1,
                // callback: function(value, index, values) {
                //   if (value % Math.round(values[0] / 6) == 0) {
                //     return value;
                //   } else if (value === 0) {
                //     return value;
                //   }
                // }
              }

              // stacked: true
            }
          ]
        },
        maintainAspectRatio: false,
        legend: {
          display: false,
          labels: {
            boxWidth: 10
          },
          position: "top"
        },
        animation: {
          duration: 2000,
          easing: "easeInOutQuart"
        }
      }
    };
  }
};
</script>
<style scoped></style>
