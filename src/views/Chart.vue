<template>
  <TestChart :chartdata="chartData" :options="chartOptions"/>
</template>

<script>
import TestChart from '@/components/TestChart'

export default {
  name: 'Chart',
  components: {
    TestChart
  },
  data() {
    return {
      chartdata: {
      labels: ['January', 'February'],
      datasets: [
        {
          label: 'Data One',
          backgroundColor: '#f87979',
          data: [40, 20]
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false
    }
    }
  }
}
</script>
