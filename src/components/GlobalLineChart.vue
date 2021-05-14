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
        backgroundColor: "rgba(6, 95, 70, 0.5)",
        borderColor: "rgba(6, 95, 70)",
        data: newCases
      }]
    },
    this.options)
  }
})
</script>