<template>
  <div class="grid md:grid-cols-3 gap-4">
    <!-- Box 1 -->
    <div class="shadow-2xl border border-gray-150 bg-gray p-5 rounded">
      <h3>Infected</h3>
      <div>
        count
      </div>
      <div>
        date
      </div>
      <div>
        Number of active cases of COVID-19
      </div>
    </div>
    <!-- Box 2 -->
    <div class="shadow-2xl border border-gray-200 bg-gray p-5 rounded">
      <h3>Recovered</h3>
      <div>
        count
      </div>
      <div>
        date
      </div>
      <div>
        Number of active cases of COVID-19
      </div>
    </div>
    <!-- Box 3 -->
    <div class="shadow-2xl border border-gray-200 bg-gray p-5 rounded">
      <h3>Deaths</h3>
      <div>
        count
      </div>
      <div>
        date
      </div>
      <div>
        Number of active cases of COVID-19
      </div>
    </div>

  </div>
</template>


<script type="text/babel">
export default {
  name: 'Cards',
  };
</script>
