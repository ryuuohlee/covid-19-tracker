<template>
  <Header />
  <div class="container">
    <Cards />
    <!--<router-link to="/">Home</router-link> | -->
  </div>
  <router-view/>
</template>

<script>
import Header from './components/Header'
import Cards from './components/Cards'

export default {
  components: {
    Header,
    Cards,
  }
}
</script>