<template>
  <v-container>
    <LineChartGenerator
      v-if="!!chartData && chartData.datasets.length && showChart"
      :data="chartData"
      :options="chartOptions"
    />
    {{ chartData.datasets }}
  </v-container>
</template>

<script>
import { Line as LineChartGenerator} from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, PointElement, LinearScale, CategoryScale  } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, LineElement, PointElement, LinearScale, CategoryScale )

const xAxisTitle = "rok";

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
      },
      yAxisTitle: String,
    },

    computed: {
      chartOptions() {
        return {
          scales: {
            x: {
              title: {
                display: true,
                text: xAxisTitle,
              }
            },
            y: {
              title: {
                display: true,
                text: this.yAxisTitle,
              }
            }
          }
        }
      }
    },

    mounted() {
      console.log('linechart mounted');
      console.log(this.chartData);
      this.showChart = true;
      console.log(!!this.chartData && this.showChart);
      console.log('debugdata:');
      console.log(this.debugChartData);
    },

    updated() {
      console.log('linechart updated');
      console.log(this.chartData);
      this.showChart = true;
      console.log(!!this.chartData && this.showChart);
      console.log(this.chartData.datasets.length);
      console.log('debugdata:');
      console.log(this.debugChartData);
    }
  }
</script>
