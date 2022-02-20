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
    console.log(this.chartLabel);
    console.log(this.chartData);
    const data = {
    labels: this.chartLabel,
    datasets: [{
        label: 'Total Cases',
        data: this.chartData,
        backgroundColor: [
        'rgb(255, 99, 132)',
        'rgb(54, 162, 235)',
        'rgb(255, 205, 86)'
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
