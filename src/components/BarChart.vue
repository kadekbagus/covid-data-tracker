<template>
    <div class="container">
        <canvas v-bind:id="chartId"></canvas>
    </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';

export default {
  name: 'BarChart',
  props: ['trendTotalConfirmed',
          'trendTotalRecovered',
          'trendTotalDeaths',
          'chartLabel',
          'chartColor', 
          'chartId',
          'chartTitle',
        ],
  mounted () {
    const data = {
    labels: this.chartLabel,
    datasets: [
        {
            label: this.chartTitle[0],
            data: this.trendTotalConfirmed,
            backgroundColor: this.chartColor[0],
            borderWidth: 1
        },
        {
            label: this.chartTitle[1],
            data: this.trendTotalRecovered,
            backgroundColor: this.chartColor[1],
            borderWidth: 1
        },
        {
            label: this.chartTitle[2],
            data: this.trendTotalDeaths,
            backgroundColor: this.chartColor[2],
            borderWidth: 1
        }
    ]
    };
    
    const config = {
        type: 'bar',
        data: data,
        options: {
        scales: {
                y: {
                    beginAtZero: true
                }
            }
        },
    };

    var barChart = new Chart(
        document.getElementById(this.chartId),
        config
    );
  }
}
</script>
