<template>
  <main v-if="!loading"> 
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats" />
  </main>

  <main v-else class="flex flex-col align-center justify-center text-center"> 
    <div class="text-gray-500 text-3xl mt-10 mb-6">fetching data...</div>
    <div class="fa-3x"><i class="fas fa-spinner fa-pulse"></i></div>
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: '',
    }
  },
  methods: {
    async fetchData() {
      const response = await fetch('https://api.covid19api.com/summary')
      const data = await response.json()
      return data
    }
  },
  async created() {
    const data = await this.fetchData()
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
