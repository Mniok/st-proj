<template>
  <v-container>
    <LineChartGenerator
      v-if="!!chartData && chartData.datasets.length && showChart"
      :data="chartData"
      :options="chartOptions"
    />
  </v-container>
</template>

<script>
import { Line as LineChartGenerator} from 'vue-chartjs';
import { Chart as ChartJS, Title, SubTitle, Tooltip, Legend, LineElement, PointElement, LinearScale, CategoryScale  } from 'chart.js';

ChartJS.register(Title, SubTitle, Tooltip, Legend, LineElement, PointElement, LinearScale, CategoryScale )

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
        },
        /*loadedChartData: {
          labels: [], //X axis
          datasets: [
            {
              label: "",
              data: [],
              backgroundColor: "",
              borderColor: "",
            }
          ],
        }*/
      }
    },

    props: {
      chartData: {
        labels: Array,
        datasets: Array,
      },
      yAxisTitle: String,
      chartTitle: String,
      chartSubtitle: String,
    },

    computed: {
      chartOptions() {
        return {
          responsive: true,
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
          },

          plugins: {
            title: {
              display: true,
              text: this.chartTitle,
            },
            subtitle: {
              display: true,
              text: this.chartSubtitle,
              padding: {
                bottom: 16,
              },
            },
            legend: {
              position: 'right',
            }
          }
        }
      }
    },

    mounted() {
      //console.log('linechart mounted');
      //console.log(this.chartData);
      this.showChart = true;
      //console.log(!!this.chartData && this.showChart);
      //console.log('debugdata:');
      //console.log(this.debugChartData);
    },

    /*updated() {
      console.log('linechart updated updated');
      //this.showChart = false;
      this.loadedChartData = this.chartData;
      console.log(this.loadedChartData);
      console.log(this.loadedChartData.datasets.length);
      this.loadedChartData = this.debugChartData;
      this.showChart = true;
      //console.log(!!this.chartData && this.showChart);
      //console.log(this.chartData.datasets.length);
      //console.log('debugdata:');
      //console.log(this.debugChartData);
      //console.log(this.chartTitle);
      //this.debugChartData.labels[2] += "oo";
      //this.debugChartData.datasets[0].data[2] *= 1.3;
    }*/
  }
</script>
