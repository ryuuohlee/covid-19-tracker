<template>
  <div class="grid grid-cols-3 gap-y-4">
    <div class="h-4">
      <div class="bg-green-600 px-4 mx-2">
        Infected
        <div class="iCountUp">
          count
        </div>
      </div>
      <div class="iCountUp">
        count
      </div>
    </div>
    <div class="text-center bg-green-600 mx-2">
      Recovered
    </div>
    <div class="text-center bg-green-600 mx-2">
      Deaths
    </div>
  </div>
</template>


<script type="text/babel">
export default {
  name: 'Cards',
  };
</script>
