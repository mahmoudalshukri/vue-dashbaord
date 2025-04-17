<template>
  <div class="dashboard">
    <h1>Priority-Based Dashboard</h1>
    
    <div class="chart-container">
      <div v-if="chart1Loaded" class="chart-wrapper">
        <h2>Line Chart (Priority 1)</h2>
        <line-chart :chart-data="chart1Data" />
      </div>
      <div v-else class="loading-placeholder">
        Loading Priority 1 Chart...
      </div>
    </div>

    <div class="chart-container">
      <div v-if="chart2Loaded" class="chart-wrapper">
        <h2>Donut Chart (Priority 2)</h2>
        <donut-chart :chart-data="chart2Data" />
      </div>
      <div v-else class="loading-placeholder">
        Waiting for Priority 1 to complete...
      </div>
    </div>

    <div class="chart-container">
      <div v-if="chart3Loaded" class="chart-wrapper">
        <h2>Column Chart (Priority 3)</h2>
        <column-chart :chart-data="chart3Data" />
      </div>
      <div v-else class="loading-placeholder">
        Waiting for Priority 2 to complete...
      </div>
    </div>
  </div>
</template>

<script>
import LineChart from './LineChart.vue';
import DonutChart from './DonutChart.vue';
import ColumnChart from './ColumnChart.vue';

export default {
  components: {
    LineChart,
    DonutChart,
    ColumnChart
  },
  data() {
    return {
      chart1Loaded: false,
      chart2Loaded: false,
      chart3Loaded: false,
      chart1Data: null,
      chart2Data: null,
      chart3Data: null
    };
  },
  mounted() {
    this.loadCharts();
  },
  methods: {
    fetchChartData(chartNumber) {
      return new Promise((resolve) => {
        const delay = 2000 + Math.random() * 3000;
        
        setTimeout(() => {
          const data = {
            labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
            datasets: [{
              label: `Dataset ${chartNumber}`,
              data: Array(6).fill().map(() => Math.floor(Math.random() * 100)),
              backgroundColor: this.getBackgroundColor(chartNumber)
            }]
          };
          resolve(data);
        }, delay);
      });
    },

    getBackgroundColor(chartNumber) {
      const colors = [
        'rgba(75, 192, 192, 0.6)',
        'rgba(255, 99, 132, 0.6)',
        'rgba(54, 162, 235, 0.6)',
        'rgba(255, 206, 86, 0.6)',
        'rgba(153, 102, 255, 0.6)',
        'rgba(255, 159, 64, 0.6)'
      ];
      return colors.slice(0, chartNumber === 2 ? 3 : 1);
    },

    async loadCharts() {
      try {
        this.chart1Data = await this.fetchChartData(1);
        this.chart1Loaded = true;
        
        this.chart2Data = await this.fetchChartData(2);
        this.chart2Loaded = true;
        
        this.chart3Data = await this.fetchChartData(3);
        this.chart3Loaded = true;
      } catch (error) {
        console.error('Error loading charts:', error);
      }
    }
  }
};
</script>

<style scoped>
.dashboard {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.chart-container {
  margin: 30px 0;
  padding: 20px;
  border: 1px solid #eee;
  border-radius: 8px;
  min-height: 300px;
}

.chart-wrapper {
  position: relative;
  height: 400px;
}

.loading-placeholder {
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #666;
  font-style: italic;
}

h1 {
  color: #2c3e50;
  margin-bottom: 30px;
}

h2 {
  color: #34495e;
  margin-bottom: 15px;
}
</style>