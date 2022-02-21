<template>
  <main v-if="!loading"> 
    <!-- <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats" /> -->
    <PieChart :chartData="pieChartTotalData" :chartLabel="pieChartTotalLabel" />
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
import PieChart from '@/components/PieChart'
import moment from 'moment'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    PieChart,
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: '',
      pieChartTotalLabel: ['Confirmed', 'Recovered', 'Deaths'],
      pieChartTotalData: [],
      pieChartTotalColor: [],
      totalConfirmed: 0,
      totalRecovered: 0,
      totalDeaths: 0,
      startDate: '',
      endDate: '',
    }
  },
  methods: {
    async fetchData(queryString) {
      const response = await fetch(queryString)
      const data = await response.json()
      return data
    },
    async getTotalCountry() {
        let totalCountryAPI = 'https://api.coronatracker.com/v3/stats/worldometer/country?countryCode=id'
        const data = await this.fetchData(totalCountryAPI)
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
    },
    getStartEndDate() {
      let now = moment();
      let pastDates = [];
      for( let i=1;i<=28;i++) { 
      now = now.subtract(1, 'day')
        // console.log(now.format('YYYY-MM-DD'))
        // pastDates.push(now)
        if (i===1) {
          this.endDate = now.format('YYYY-MM-DD')
        }

        if (i===28) {
          this.startDate = now.format('YYYY-MM-DD')
        }
      }
    },
  },
  async created() {
    this.getStartEndDate()
    let totalCountryData = await this.getTotalCountry()
    this.totalConfirmed = totalCountryData[0].totalConfirmed
    this.totalRecovered = totalCountryData[0].totalRecovered
    this.totalDeaths = totalCountryData[0].totalDeaths
    this.pieChartTotalData.push(this.totalConfirmed)
    this.pieChartTotalData.push(this.totalRecovered)
    this.pieChartTotalData.push(this.totalDeaths)
    // this.dataDate = data.Date
    // this.stats = data.Global
    // this.countries = data.Countries
    this.loading = false
  }
}
</script>
