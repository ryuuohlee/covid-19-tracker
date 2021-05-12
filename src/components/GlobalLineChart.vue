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
    const dates = this.dates;
    const totals = this.counts;

    this.renderChart({
      labels: dates,
      datasets: [{
        label: this.label,
        data: totals
      }]
    },
    this.options)
  }
})
</script>