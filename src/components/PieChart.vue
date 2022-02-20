<template>
    <div>
        <canvas id="myChart"></canvas>
    </div>
</template>

<script>
//import { Line } from 'chart.js'
import { Chart, registerables } from 'chart.js';

export default {
  name: 'PieChart',
  props: ['chartData', 'chartLabel'],
  mounted () {
    const data = {
    labels: this.chartLabel,
    datasets: [{
        label: 'Total Cases',
        data: this.chartData,
        backgroundColor: [
        'red',
        'green',
        'black'
        ],
        hoverOffset: 4
    }]
    };
    
    const config = {
        type: 'pie',
        data: data,
    };

    var myChart = new Chart(
        document.getElementById('myChart'),
        config
    );
  }
}
</script>
