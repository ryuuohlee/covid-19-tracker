<template>
  <main v-if="!loading">
    {{ this.title}}
    <Cards :dataDate="dataDate" :cases="cases" :totalRecovered="totalRecovered" :totalDeaths="totalDeaths" />
    <CountrySelect @get-country="getCountryData" :countries=this.countries />
  </main>

  <main class="flex flex-col align-center justify-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>

<script>
// @ is an alias to /src
import Cards from '../components/Cards'
import CountrySelect from '../components/CountrySelect'

export default {
  name: 'Home',
  components: {
    Cards,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      loadingImage: require('../assets/Infinity-1s-200px.gif'),
      title: 'Global',
      countries: 'All',
      dataDate: '',
      totalCases: '',
      totalRecovered: '',
      totalDeaths: ''
    }
  },
  methods:{
    async fetchCovidData(){
      // let res;
      // this.country === 'All' ? res = await fetch('https://corona.lmao.ninja/v3/covid-19/all') : res = await fetch('https://corona.lmao.ninja/v3/covid-19/countries')
      const res = await fetch('https://corona.lmao.ninja/v3/covid-19/countries')
      const data = await res.json()

      return data
    },
    getCountryData(country) {

      this.title = country
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    const reducer = (accum, curr) => accum + curr

    this.dataDate = data[0].updated
    this.countries = data.map(countries => countries.country)
    this.cases = data.map(countries => countries.cases)
    this.recovered = data.map(countries => countries.recovered)
    this.deaths = data.map(countries => countries.deaths)
    this.loading = false
  },
  filtered() {
    if(title === 'Global') {
      const reducer = (accum, curr) => accum + curr

      this.cases = this.cases.reduce(reducer)
    }
  }
}
</script>
