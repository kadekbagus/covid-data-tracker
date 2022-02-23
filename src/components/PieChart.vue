<template>
    <div class="container">
        <canvas id="pieChart"></canvas>
    </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';

export default {
  name: 'PieChart',
  props: ['chartData', 'chartLabel', 'chartColor'],
  mounted () {
    const data = {
    labels: this.chartLabel,
    datasets: [{
        label: 'Total Cases',
        data: this.chartData,
        backgroundColor: this.chartColor,
        hoverOffset: 4
    }]
    };
    
    const config = {
        type: 'pie',
        data: data,
    };

    var pieChart = new Chart(
        document.getElementById('pieChart'),
        config
    );
  }
}
</script>


<style scoped>
/* .container {
    max-width: 500px;
    max-height: 500px;
} */
</style>