<template>
  <div>
      <h3 class="text-xl font-bold mb-2">Live Infected Cases by Country</h3>
      <div class="overflow-y-auto" style="height: 600px;">
        <table class="table-fixed">
          <tr>
            <th class="w-1/3 text-left">Countries</th>
            <th class="text-right">Infected Cases</th>
          </tr>
          <tr
            v-for="(country, index) in data"
            :value="country"
            :key="country"
            :style="index%2===0 ? {'background-color': 'rgb(211,211,211)'} : {'background-color': 'white'}">
            <td>{{ country.country }} </td>
            <td class="text-right font-bold">{{ numberWithCommas(country.cases) }} </td>
          </tr>
        </table>
      </div>
    </div>
</template>

<script>
export default {
  name: 'CountriesTable',
  props: ['data'],
  // data() {
  //   return {
  //     selected: 0
  //   }
  // },
  methods: {
    numberWithCommas(x) {
        x = x.toString();
        var pattern = /(-?\d+)(\d{3})/;
        while (pattern.test(x))
            x = x.replace(pattern, "$1,$2");
        return x;
    },
  // for future dynamic chart
  // onSelect() {
  //     const country = this.countries.find((item) => item === this.selected)

  //     this.$emit('get-country', country)
  //   }
  }
}
</script>