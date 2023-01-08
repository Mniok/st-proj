<template>
  <v-container>
    <LineChartGenerator
      v-if="!!chartData && showChart"
      :data="debugChartData"
    />
  </v-container>
</template>

<script>
	import { Line as LineChartGenerator} from 'vue-chartjs';
  import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, PointElement, LinearScale, CategoryScale  } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, LineElement, PointElement, LinearScale, CategoryScale )

  export default {
    name: 'LineChart',

    components: {
      LineChartGenerator
    },

    data() {
      return {
        showChart: false,
        debugChartData: {
          labels: ['ay', 'bee', 'cee', 'dee'], //X axis
          datasets: [
            {
              label: "Data One",
              data: [1, 28, 3, 42]
            }
          ],
          // datasets: [{ label:"", data:[], fill:bool, borderColor:"", tension: 0.1 }]
        }
      }
    },

    props: {
      chartData: {
        labels: Array,
        datasets: Array,
      }
    },

    mounted() {
      console.log('linechart mounted');
      console.log(this.chartData);
      this.showChart = true;
      console.log(!!this.chartData && this.showChart);
      console.log('debugdata:');
      console.log(this.debugChartData);
    }
  }
</script>
