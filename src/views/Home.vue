<template>
  <main v-if="!loading">
    <CountrySelect @get-country="updateData" :countries="countries"/>

    <div class="grid grid-cols-2 mt-10">
      <InfoBox :totalConfirmed="totalConfirmed" :totalRecovered="totalRecovered" :totalDeaths="totalDeaths" :lastUpdated="lastUpdated" :countryName="countryName"/>
      <PieChart :chartData="pieChartTotalData" :chartLabel="pieChartTotalLabel" :chartColor="pieChartTotalColor"/>
    </div>

    <div class="grid grid-cols-1">
      <DataBoxes :deathRate="deathRate" :recoveryRate="recoveryRate"/>
    </div>
   
    <div class="grid grid-cols-3 mt-10">
      <LineChart :chartData="newCaseConfirm " :chartLabel="newCaseLabel" :chartColor="lineChartColor[0]" :chartId="lineChartId[0]" :chartTitle="lineChartTitle[0]"/>
      <LineChart :chartData="newCaseRecover" :chartLabel="newCaseLabel" :chartColor="lineChartColor[1]" :chartId="lineChartId[1]" :chartTitle="lineChartTitle[1]"/>
      <LineChart :chartData="newCaseDeath" :chartLabel="newCaseLabel" :chartColor="lineChartColor[2]" :chartId="lineChartId[2]" :chartTitle="lineChartTitle[2]"/>
    </div>

    <div class="grid grid-cols-1 mt-10">
      <BarChart :trendTotalConfirmed="trendTotalConfirmed" 
                :trendTotalRecovered="trendTotalRecovered" 
                :trendTotalDeaths="trendTotalDeaths"
                :chartId="barChartId"
                :chartTitle="trendTotalTitle"
                :chartLabel="trendTotalLabel"
                :chartColor="trendTotalColor"
      />
    </div>

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
import BarChart from '@/components/BarChart'
import InfoBox from '@/components/InfoBox'
import moment from 'moment'
import Countries from './../country'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    PieChart,
    LineChart,
    BarChart,
    InfoBox,
  },
  data() {
    return {
      loading: true,
      countries: Countries,
      pieChartTotalLabel: ['Confirmed', 'Recovered', 'Deaths'],
      pieChartTotalData: [],
      pieChartTotalColor: ['#ef4444', '#22c55e', '#6B7280'],
      totalConfirmed: 0,
      totalRecovered: 0,
      totalDeaths: 0,
      deathRate: 0,
      recoveryRate: 0,
      startDate: '',
      endDate: '',
      countryName: 'Indonesia',
      countryCode: 'id',
      newCaseLabel: [],
      newCaseDeath: [],
      newCaseConfirm: [],
      newCaseRecover: [],
      lineChartColor: ['#ef4444', '#22c55e', '#6B7280'],
      lineChartId: ['lineChart1', 'lineChart2', 'lineChart3'],
      lineChartTitle: ['Daily Confirm Cases', 'Daily Recovery Cases', 'Daily Death Cases'],
      lastUpdated: '',
      barChartId: ['barChart1'],
      trendTotalTitle: ['Total Confirmed', 'Total Recovered', 'Total Deaths'],
      trendTotalColor: ['#ef4444', '#22c55e', '#6B7280'],
      trendTotalLabel: [],
      trendTotalConfirmed: [],
      trendTotalRecovered: [],
      trendTotalDeaths: [],
    }
  },
  methods: {
    async fetchData(queryString) {
      const response = await fetch(queryString)
      const data = await response.json()
      return data
    },
    async getTotalCountry() {
        let totalCountryAPI = `https://api.coronatracker.com/v3/stats/worldometer/country?countryCode=${this.countryCode}`
        const data = await this.fetchData(totalCountryAPI)
        return data
    },
    async getDailyNewCases() {
        let dailyCaseAPI = `https://api.coronatracker.com/v5/analytics/newcases/country?countryCode=${this.countryCode}&startDate=${this.startDate}&endDate=${this.endDate}`
        const data = await this.fetchData(dailyCaseAPI)
        return data
    },
    async getTrendTotalCases() {
        let dailyCaseAPI = `https://api.coronatracker.com/v5/analytics/trend/country?countryCode=${this.countryCode}&startDate=${this.startDate}&endDate=${this.endDate}`
        const data = await this.fetchData(dailyCaseAPI)
        return data
    },
    async updateData(country) {
      this.countryName = country.Country
      this.countryCode = country.ID
      this.pieChartTotalData = []
      this.newCaseLabel = []
      this.newCaseConfirm = []
      this.newCaseDeath = []
      this.newCaseRecover = []
      this.trendTotalLabel = []
      this.trendTotalConfirmed = []
      this.trendTotalRecovered = []
      this.trendTotalDeaths = []
      this.loading = true
      this.getStartEndDate()
      let totalCountryData = await this.getTotalCountry()
      this.totalConfirmed = totalCountryData[0].totalConfirmed
      this.totalRecovered = totalCountryData[0].totalRecovered
      this.totalDeaths = totalCountryData[0].totalDeaths
      this.lastUpdated = moment(totalCountryData[0].lastUpdated).format('DD MMMM YYYY')
      this.pieChartTotalData.push(this.totalConfirmed)
      this.pieChartTotalData.push(this.totalRecovered)
      this.pieChartTotalData.push(this.totalDeaths)
      this.calculateDeathRate()
      this.calculateRecoveryRate()
      let dailyNewCaseData = await this.getDailyNewCases()
      let maxData = (dailyNewCaseData.length)-1
      for( let i=0; i<=maxData; i++) {
        this.newCaseLabel.push(moment(dailyNewCaseData[i].last_updated).format('DD MMM'))
        this.newCaseConfirm.push(dailyNewCaseData[i].new_infections)
        this.newCaseDeath.push(dailyNewCaseData[i].new_deaths)
        this.newCaseRecover.push(dailyNewCaseData[i].new_recovered)
      }
      let trendTotalCaseData = await this.getTrendTotalCases()
      maxData = (trendTotalCaseData.length)-1
      for( let i=0; i<=maxData; i++) {
        this.trendTotalLabel.push(moment(trendTotalCaseData[i].last_updated).format('DD MMM'))
        this.trendTotalConfirmed.push(trendTotalCaseData[i].total_confirmed)
        this.trendTotalRecovered.push(trendTotalCaseData[i].total_recovered)
        this.trendTotalDeaths.push(trendTotalCaseData[i].total_deaths)
      }
      this.loading = false
    },
    getStartEndDate() {
      let now = moment();
      for( let i=1; i<=28; i++) { 
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
    },
  },
  async created() {
    this.getStartEndDate()
    let totalCountryData = await this.getTotalCountry()
    this.totalConfirmed = totalCountryData[0].totalConfirmed
    this.totalRecovered = totalCountryData[0].totalRecovered
    this.totalDeaths = totalCountryData[0].totalDeaths
    this.lastUpdated = moment(totalCountryData[0].lastUpdated).format('DD MMMM YYYY')
    this.pieChartTotalData.push(this.totalConfirmed)
    this.pieChartTotalData.push(this.totalRecovered)
    this.pieChartTotalData.push(this.totalDeaths)
    this.calculateDeathRate()
    this.calculateRecoveryRate()
    let dailyNewCaseData = await this.getDailyNewCases()
    let maxData = (dailyNewCaseData.length)-1
    for( let i=0; i<=maxData; i++) {
      this.newCaseLabel.push(moment(dailyNewCaseData[i].last_updated).format('DD MMM'))
      this.newCaseConfirm.push(dailyNewCaseData[i].new_infections)
      this.newCaseDeath.push(dailyNewCaseData[i].new_deaths)
      this.newCaseRecover.push(dailyNewCaseData[i].new_recovered)
    }
    let trendTotalCaseData = await this.getTrendTotalCases()
    maxData = (trendTotalCaseData.length)-1
    for( let i=0; i<=maxData; i++) {
      this.trendTotalLabel.push(moment(trendTotalCaseData[i].last_updated).format('DD MMM'))
      this.trendTotalConfirmed.push(trendTotalCaseData[i].total_confirmed)
      this.trendTotalRecovered.push(trendTotalCaseData[i].total_recovered)
      this.trendTotalDeaths.push(trendTotalCaseData[i].total_deaths)
    }
    this.loading = false
  }
}
</script>
