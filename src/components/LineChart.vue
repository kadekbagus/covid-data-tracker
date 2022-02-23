<template>
    <div class="container">
        <canvas v-bind:id="chartId"></canvas>
    </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';

export default {
  name: 'LineChart',
  props: ['chartData', 
          'chartLabel',
          'chartColor', 
          'chartId',
          'chartTitle',
        ],
  mounted () {
    const data = {
    labels: this.chartLabel,
    datasets: [{
        label: this.chartTitle,
        data: this.chartData,
        fill: false,
        borderColor: this.chartColor,
        tension: 0.1
    }]
    };
    
    const config = {
        type: 'line',
        data: data,
    };

    var lineChart = new Chart(
        document.getElementById(this.chartId),
        config
    );
  }
}
</script>


<style scoped>
.container {
    max-width: 500px;
    max-height: 500px;
}
</style>