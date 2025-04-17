<template>
  <canvas ref="chartCanvas"></canvas>
</template>

<script>
import { Chart, registerables } from 'chart.js';

export default {
  props: {
    chartData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      chartInstance: null
    };
  },
  mounted() {
    Chart.register(...registerables);
    this.renderChart();
  },
  watch: {
    chartData: {
      handler() {
        this.renderChart();
      },
      deep: true
    }
  },
  methods: {
    renderChart() {
      if (this.chartInstance) {
        this.chartInstance.destroy();
      }

      if (this.$refs.chartCanvas && this.chartData) {
        this.chartInstance = new Chart(this.$refs.chartCanvas, {
          type: 'doughnut',
          data: this.chartData,
          options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: '70%'
          }
        });
      }
    }
  },
  beforeDestroy() {
    if (this.chartInstance) {
      this.chartInstance.destroy();
    }
  }
};
</script>

<style scoped>
canvas {
  width: 100%;
  height: 100%;
}
</style>