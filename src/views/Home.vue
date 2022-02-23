<template>
  <main v-if="!loading"> 
    <!-- <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats" /> -->
    <PieChart :chartData="pieChartTotalData" :chartLabel="pieChartTotalLabel" :chartColor="pieChartTotalColor"/>
    <LineChart :chartData="newCaseConfirm " :chartLabel="newCaseLabel" :chartColor="lineChartColor[0]" :chartId="lineChartId[0]" :chartTitle="lineChartTitle[0]"/>
    <LineChart :chartData="newCaseRecover" :chartLabel="newCaseLabel" :chartColor="lineChartColor[1]" :chartId="lineChartId[1]" :chartTitle="lineChartTitle[1]"/>
    <LineChart :chartData="newCaseDeath" :chartLabel="newCaseLabel" :chartColor="lineChartColor[2]" :chartId="lineChartId[2]" :chartTitle="lineChartTitle[2]"/>
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
import LineChart from '@/components/LineChart'
import moment from 'moment'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    PieChart,
    LineChart,
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
      pieChartTotalColor: ['#be2528', '#2596be', '#0b2d39'],
      totalConfirmed: 0,
      totalRecovered: 0,
      totalDeaths: 0,
      deathRate: 0,
      recoveryRate: 0,
      startDate: '',
      endDate: '',
      country: 'id',
      newCaseLabel: [],
      newCaseDeath: [],
      newCaseConfirm: [],
      newCaseRecover: [],
      lineChartColor: ['#be2528', '#2596be', '#0b2d39'],
      lineChartId: ['lineChart1', 'lineChart2', 'lineChart3'],
      lineChartTitle: ['Daily Confirm Cases', 'Daily Recovery Cases', 'Daily Death Cases'],
    }
  },
  methods: {
    async fetchData(queryString) {
      const response = await fetch(queryString)
      const data = await response.json()
      return data
    },
    async getTotalCountry() {
        let totalCountryAPI = `https://api.coronatracker.com/v3/stats/worldometer/country?countryCode=${this.country}`
        const data = await this.fetchData(totalCountryAPI)
        return data
    },
    async getDailyNewCases() {
        let dailyCaseAPI = `https://api.coronatracker.com/v5/analytics/newcases/country?countryCode=${this.country}&startDate=${this.startDate}&endDate=${this.endDate}`
        const data = await this.fetchData(dailyCaseAPI)
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
        if (i===1) {
          this.endDate = now.format('YYYY-MM-DD')
        }
        if (i===28) {
          this.startDate = now.format('YYYY-MM-DD')
        }
      }
    },
    calculateDeathRate() {
      let num = Number((this.totalDeaths/this.totalConfirmed)*100) 
      let roundedString = num.toFixed(1)
      let rounded = Number(roundedString)
      this.deathRate = rounded
    },
    calculateRecoveryRate() {
      let num = Number((this.totalRecovered/this.totalConfirmed)*100) 
      let roundedString = num.toFixed(1)
      let rounded = Number(roundedString)
      this.recoveryRate = rounded
    }
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
    this.calculateDeathRate()
    this.calculateRecoveryRate()
    let dailyNewCaseData = await this.getDailyNewCases()
    for( let i=0; i<=26; i++) {
      this.newCaseLabel.push(moment(dailyNewCaseData[i].last_updated).format('DD MMM'))
      this.newCaseConfirm.push(dailyNewCaseData[i].new_infections)
      this.newCaseDeath.push(dailyNewCaseData[i].new_deaths)
      this.newCaseRecover.push(dailyNewCaseData[i].new_recovered)
    }
    this.loading = false
  }
}
</script>
