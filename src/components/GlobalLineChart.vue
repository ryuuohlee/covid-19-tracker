<script>
import { defineComponent } from 'vue'
import { Line } from 'vue3-chart-v2'

export default defineComponent({
  extends: Line,
  props:{
    label: {
      type: String
    },
    chartData: {
      type: Object
    },
    options: {
      type: Object
    }
  },
  mounted () {
    let previousDayData;
    let newCases = [];
    const totals = Object.values(this.chartData.cases);

    //create data for newCases only
    for(let date in this.chartData.cases) {
      if(previousDayData){
        let newData = this.chartData.cases[date] - previousDayData;
        newCases.push(newData);
      }
      previousDayData = this.chartData.cases[date]
    }

    this.renderChart({
      labels: Object.keys(this.chartData.cases),
      datasets: [{
        label: this.label,
        data: newCases
      }]
    },
    this.options)
  }
})
</script>