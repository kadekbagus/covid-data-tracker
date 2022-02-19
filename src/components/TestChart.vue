<template>
    <div>
        <canvas id="myChart"></canvas>
    </div>
</template>

<script>
import { Line } from 'chart.js'
import { Chart, registerables } from 'chart.js';

const labels = [
  'January',
  'February',
  'March',
  'April',
  'May',
  'June',
];
const data = {
  labels: labels,
  datasets: [{
    label: 'My First dataset',
    backgroundColor: 'rgb(255, 99, 132)',
    borderColor: 'rgb(255, 99, 132)',
    data: [0, 10, 5, 2, 20, 30, 45],
  }]
};

const config = {
  type: 'line',
  data,
  options: {}
};

export default {
  extends: Line,
  props: {
    chartdata: {
      type: Object,
      default: null
    },
    options: {
      type: Object,
      default: null
    }
  },
  mounted () {
      var myChart = new Chart(
        document.getElementById('myChart'),
        config
    );
  }
}
</script>
