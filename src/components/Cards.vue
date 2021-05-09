<template>
  <div class="grid md:grid-cols-3 gap-4">
    <!-- Box 1 -->
    <div class="shadow-xl border border-gray-150 bg-gray p-5 rounded">
      <h3 class="text-2xl text-green-800 font-bold mb-2">Infected</h3>
      <div>
        {{ numberWithCommas(infected) }}
      </div>
      <div class="font-bold">
        Number of active cases
      </div>
    </div>
    <!-- Box 2 -->
    <div class="shadow-xl border border-gray-200 bg-gray p-5 rounded">
      <h3 class="text-2xl text-blue-800 font-bold mb-2">Recovered</h3>
      <div>
        {{ numberWithCommas(recovered) }}
      </div>
      <div class="font-bold">
        Number of recovered cases
      </div>
    </div>
    <!-- Box 3 -->
    <div class="shadow-xl border border-gray-200 bg-gray p-5 rounded">
      <h3 class="text-2xl text-red-800 font-bold mb-2">Deaths</h3>
      <div>
        {{ numberWithCommas(deaths) }}
      </div>
      <div class="font-bold">
        Number of deaths
      </div>
    </div>

  </div>
</template>


<script type="text/babel">
export default {
  name: 'Cards',
  props: ['dataDate', 'infected', 'recovered', 'deaths'],
  methods: {
    numberWithCommas(x) {
        x = x.toString();
        var pattern = /(-?\d+)(\d{3})/;
        while (pattern.test(x))
            x = x.replace(pattern, "$1,$2");
        return x;
    }
  }
}
</script>
