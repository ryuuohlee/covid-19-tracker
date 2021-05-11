<template>
  <main class="flex flex-col md:flex-row justify-evenly" v-if="!loading">
    <div class="app_left">
      <!-- title and updated date -->
      <div class="text-center">
        <h2 class="text-3xl font-bold"> {{ this.title }} </h2>
        <div class="text-xl mt-2 mb-10"><span class="font-bold">Updated</span> {{ timestamp }} </div>
      </div>
      <!-- info cards -->
      <Cards :infected="infected" :recovered="recovered" :deaths="deaths" />
      <!-- country select dropdown and button -->
      <div class="flex">
        <CountrySelect @get-country="getCountryData" :countries="countries" :title="title"/>
        <button
          @click="clearCountryData"
          v-if="this.title !== 'Global'"
          class="bg-red-700 text-white rounded p-3 mt-10 ml-4 focus:outline-none hover:bg-red-600">
          Clear Country
        </button>
      </div>
    </div>
    <!-- country select dropdown and button -->
    <div class="app_right h-auto mt-10 mb-10 md:ml-4 md:mt-0 ">
      <div class="shadow-xl border border-gray-150 bg-gray p-5 rounded">
        <CountriesTable :data="data.sort((a,b) => b.cases-a.cases)" />
        <h3 class="text-xl font-bold mt-4 mb-2">Global chart</h3>
        <CountriesLineChart :globalCases="globalCases" />
      </div>
    </div>
  </main>

  <!-- loading screen -->
  <main class="flex flex-col align-center justify-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>

<script>
import Cards from '../components/Cards'
import CountrySelect from '../components/CountrySelect'
import CountriesTable from '../components/CountriesTable'
import CountriesLineChart from '../components/CountriesLineChart'
import moment from 'moment'

export default {
  name: 'Home',
  components: {
    Cards,
    CountrySelect,
    CountriesTable,
    CountriesLineChart
  },
  computed: {
    timestamp: function() {
      return moment(this.dataDate).format('MMMM Do YYYY, h:mm:ss a')
    }
  },
  data() {
    return {
      loading: true,
      loadingImage: require('../assets/Infinity-1s-200px.gif'),
      title: 'Global',
      countries: '',
      dataDate: '',
      totalCases: '',
      totalRecovered: '',
      totalDeaths: '',
      globalCases: [],
      dates: [],
      counts: []
    }
  },
  methods:{
    async fetchCovidData(){
      const res = await fetch('https://corona.lmao.ninja/v3/covid-19/countries')
      const data1 = await res.json()

      return data1
    },
    async fetchGlobalCovidData(){
      const res = await fetch('https://corona.lmao.ninja/v3/covid-19/historical/all?lastdays=90')
      const data2 = await res.json()

      return data2
    },
    getCountryData(country) {
      this.title = country

      let index = this.countries.indexOf(country)

      this.infected = this.data[index].cases
      this.recovered = this.data[index].recovered
      this.deaths = this.data[index].deaths
    },
    async clearCountryData() {
      const reducer = (accum, curr) => accum + curr
      this.loading = true
      const data1 = await this.fetchCovidData()
      const data2 = await this.fetchGlobalCovidData()

      this.title = 'Global'
      this.dataDate = data1[0].updated
      this.countries = data1.map(countries => countries.country)
      this.infected = data1.map(countries => countries.cases).reduce(reducer)
      this.recovered = data1.map(countries => countries.recovered).reduce(reducer)
      this.deaths = data1.map(countries => countries.deaths).reduce(reducer)
      this.loading = false

      this.dates = Object.keys(data2.cases)
      this.counts = Object.values(data2.cases)

      for(let i = 0; i < data2.cases.length; i++) {
        this.globalCases.push({
          dates: this.dates[i],
          counts: this.counts[i]
        })
      }
      }
    },
    async created() {
      const data1 = await this.fetchCovidData()
      const data2 = await this.fetchGlobalCovidData()
      const reducer = (accum, curr) => accum + curr

      this.data = data1
      this.dataDate = data1[0].updated
      this.countries = data1.map(countries => countries.country)
      this.infected = data1.map(countries => countries.cases).reduce(reducer)
      this.recovered = data1.map(countries => countries.recovered).reduce(reducer)
      this.deaths = data1.map(countries => countries.deaths).reduce(reducer)

      this.dates = Object.keys(data2.cases)
      this.counts = Object.values(data2.cases)

      function createChartData(dates, counts) {
        let chartData = []
        for(let i = 0; i  < dates.length; i++) {
          chartData.push({
            date: dates[i],
            count: counts[i]
          })
        }
        return chartData
      }

      this.globalCases = createChartData(this.dates, this.counts)
      this.loading = false


    }
}
</script>
