<template>
  <main class="flex flex-col md:flex-row justify-evenly" v-if="!loading">
    <div class="app_left">
      <div class="text-center">
        <h2 class="text-3xl font-bold"> {{ this.title }} </h2>
        <div class="text-xl mt-2 mb-10"><span class="font-bold">Updated</span> {{ timestamp }} </div>
      </div>
      <Cards :infected="infected" :recovered="recovered" :deaths="deaths" />
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
    <div class="app_right mt-10 mb-10 md:ml-4 md:mt-0 ">
      <CountriesTable :data="data.sort((a,b) => b.cases-a.cases)" />
    </div>
  </main>

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
import moment from 'moment'

export default {
  name: 'Home',
  components: {
    Cards,
    CountrySelect,
    CountriesTable
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
      totalDeaths: ''
    }
  },
  methods:{
    async fetchCovidData(){
      const res = await fetch('https://corona.lmao.ninja/v3/covid-19/countries')
      const data = await res.json()

      return data
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
      const data = await this.fetchCovidData()

      this.title = 'Global'
      this.dataDate = data[0].updated
      this.countries = data.map(countries => countries.country)
      this.infected = data.map(countries => countries.cases).reduce(reducer)
      this.recovered = data.map(countries => countries.recovered).reduce(reducer)
      this.deaths = data.map(countries => countries.deaths).reduce(reducer)
      this.loading = false
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    const reducer = (accum, curr) => accum + curr

    this.data = data
    this.dataDate = data[0].updated
    this.countries = data.map(countries => countries.country)
    this.infected = data.map(countries => countries.cases).reduce(reducer)
    this.recovered = data.map(countries => countries.recovered).reduce(reducer)
    this.deaths = data.map(countries => countries.deaths).reduce(reducer)
    this.loading = false
  }
}
</script>
