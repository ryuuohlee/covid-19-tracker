<template>
  <select
    @change="onChange"
    v-model="selected"
    class="form-select mt-10 block w-full border p-3 rounded"
    :style="title!=='Global'?{'width': '83.25%'}:{'width':'100%'}">
    <option value="0">Select Country</option>
    <option v-for="country in countries" :value="country" :key="country">{{ country }}</option>
  </select>
</template>

<script>
export default {
  name: 'CountrySelect',
  props: ['countries', 'title'],
  data() {
    return {
      selected: 0
    }
  },
  methods: {
    onChange() {
      const country = this.countries.find((item) => item === this.selected)

      this.$emit('get-country', country)
    }
  }
}
</script>