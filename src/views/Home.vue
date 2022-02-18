<template>
  <main v-if="!loading"> 
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="updateData" :countries="countries"/>

    <button v-if="stats.Country" v-on:click="clearData" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      clear country
    </button>
  </main>

  <main v-else class="flex flex-col align-center justify-center text-center"> 
    <div class="text-gray-500 text-3xl mt-10 mb-6">fetching data...</div>
    <div class="fa-3x"><i class="fas fa-spinner fa-pulse"></i></div>
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
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
    },
    updateData(country) {
      console.log(country)
      this.stats = country
      this.title = country.Country
    },
    async clearData() {
      this.loading = true
      const data = await this.fetchData()
      this.title = 'Global'
      this.stats = data.Global
      this.loading = false
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
