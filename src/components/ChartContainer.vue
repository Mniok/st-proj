<template>
  <v-container>
    <br><br>
    <p>{{ metadata.name }}</p>
    <LineChart :chartData="chartDataProcessed"/>
    <span>test</span>
  </v-container>
</template>

<script>
import LineChart from './LineChart';

  export default {
    name: 'ChartContainer',

    props: ['metadata', 'chartData'],
    //metadata: "nazwa", "definicja", "cel", "priorytet", "jednostka", "metodologia", "wymiary", "zrodlo", "uwagi", "czestotliwosc"

    components: {
      LineChart,
    },

    data: () => ({
      showChart: false,
      chartDataProcessed: {
        labels: [], //X axis
        datasets: [],
        // datasets: [{ label:"", data:[], fill:bool, borderColor:"", tension: 0.1 }]
      }
    }),
	
	  mounted() {
      console.log('chartcontainer mounted (data not in props yet btw)');
	  },

    updated() {
      console.log('chartcontiner updated');
      this.showChart = false;
      console.log(this.metadata);
      console.log(this.chartData);
      console.log(',');
      this.chartDataProcessed.labels = Object.keys(Object.values(this.chartData)[0]);  //get Y axis labels like ["2011", "2012", ...]
      //var datasetLabels = Object.keys(this.chartData); //like ["kobiety", "mężczyźni"]
      console.log(this.chartDataProcessed.labels);
      var datasetsData = Object.entries(this.chartData); //with keys included to be used as dataset labels
      console.log(datasetsData);

      datasetsData.forEach(dataset => {
        console.log(dataset);
        var newDataset = {
          label: dataset[0],
          data: Object.values(dataset[1]),
        };
        this.chartDataProcessed.datasets.push(newDataset);
      });

      console.log("assigned values to chartDataProcessed");
      console.log(this.chartDataProcessed);
      this.showChart=true;
    }
  }
</script>


// SPECIFICATION
// plan - implement krajowe zamieszkanie, check globalne after finished since they are structured the same, if works - include switch for krajowe / globalne i krajowe
// chart has:
// select for wskaźnik identifier eg. 3-1-f - with select text being metadane.nazwa eg. "3.1.f Liczba zgonów w wyniku przewlekłej choroby dróg oddechowych na 100 tys. ludności",
// shows other metadata too
// shows "jednostka" on graph Y axis
// shows years on graph X axis
// 2nd select for graph in "dane" for a given wskaźnik eg. "wg płci"
// graph has a seperate line for every object in that eg. "ogółem", "mężczyźni", "kobiety"
// (which are arrays of datapoints with year as key and number as value, typically %)
// if "jednostka" contains %, scale goes up to 100%
// graph line colors depend on line key name eg. "kobiety" is pinkish, "mężczyźni" is blue, "ogółem" is dark green
// for any unusual titles not in list - uses a progression of colors based on line number
// side panel on right contains checkboxes - allows for toggling visibility of lines