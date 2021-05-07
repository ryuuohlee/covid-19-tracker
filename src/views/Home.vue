<template>
  <main v-if="!loading">
    <Cards :dataDate="dataDate" :totalCases="totalCases" :totalRecovered="totalRecovered" :totalDeaths="totalDeaths" />
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

export default {
  name: 'Home',
  components: {
    Cards,
  },
  data() {
    return {
      loading: true,
      loadingImage: require('../assets/Infinity-1s-200px.gif'),
      title: 'Global',
      dataDate: '',
      totalCases: '',
      totalRecovered: '',
      totalDeaths: ''
    }
  },
  methods:{
    async fetchCovidData(){
      const res = await fetch('https://corona.lmao.ninja/v3/covid-19/all')
      const data = await res.json()
      return data
    }
  },
  async created() {
    const data = await this.fetchCovidData()

    this.dataDate = data.updated
    this.totalCases = data.cases
    this.totalRecovered = data.recovered
    this.totalDeaths = data.deaths
    this.loading = false
  }
}
</script>
