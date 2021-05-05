<template>
  <Header />
  <div id="nav">
    <router-link to="/">Home</router-link> |
  </div>
  <router-view/>
</template>

<script>
import Header from './components/Header'

export default {
  components: {
    Header,
  }
}
</script>