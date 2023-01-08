<template>
  <v-container>
    <LineChart
      :chartData="chartDataProcessed"
      :yAxisTitle="metadata.jednostka"
      :chartTitle="metadata.nazwa"
      :chartSubtitle="selectedDatasetName"
    />
  </v-container>
</template>

<script>
import LineChart from './LineChart';

  export default {
    name: 'ChartContainer',

    props: ['metadata', 'chartData', 'selectedDatasetName'],
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
      },
    }),

    methods: {
      calculateDataColor(labelName, isDot){
        // returns slightly intenser color for dot than for line
        // set values for certain label name, fallback chooses color based on ordinal number of dataset
        const colors = [
          "#e53935", //red darken-1
          "#43a047", //green darken-1
          "#1e88e5", //blue darken-1
          "#f57c00", //orange darken-1
          "#c0ca33", //lime darken-1 (yellowish)
          "#00acc1", //cyan darken-1
          "#ff4081", //pink accent-1
          "#3949ab", //indigo darken-1
          "#5e35b1", //deep-purple darken-1
          "#00897b", //teal darken-1
          "#ff3d00", //deep-orange accent-3
          "#ffd54f", //amber lighten-2
          "#546e7a", //blue-grey darken-2
          "#6d4c41", //brown darken-1
          "#000000", //black
        ]

        switch (labelName) {
          //"wg płci"
          case "kobiety": return isDot ? "#ff33cc" : "#ff66cc"; //pink
          case "mężczyźni": return isDot ? "#0066cc" : "#0099ff"; //blue
          case "ogółem": return isDot ? "#2eb82e" : "#5cd65c"; //green
          //"miasto/wieś"
          case "miasto": return isDot ? "#546e7a" : "#78909c"; //blue-gray
          case "wieś": return isDot ? "#996633" : "#cc9900"; //light brown

          /*
          //"wg grup wieku"
          case "15-19 lat": return "#ff66cc";
          case "20-29 lat": return "#ff66cc";
          case "30-39 lat": return "#ff66cc";
          case "40-49 lat": return "#ff66cc";
          case "50-59 lat": return "#ff66cc";
          case "60-69 lat": return "#ff66cc";
          case "70-79 lat": return "#ff66cc";
          case "80 lat i więcej": return "cc6699";
          case "0-4 lata": return "#ff66cc";
          case "5-9 lat": return "#ff66cc";
          case "10-14 lat": return "#ff66cc";
          //"wg grup wieku"
          case "w wieku 16-24 lata": return "#ff66cc";
          case "w wieku 25-34 lata": return "#ff66cc";
          case "w wieku 35-44 lata": return "#ff66cc";
          case "w wieku 45-54 lata": return "#ff66cc";
          case "w wieku 55-64 lata": return "#ff66cc";
          case "w wieku 65-74 lata": return "#ff66cc";
          //"wg grup wieku"
          case "osoby w wieku 0-17 lat": return "#ff66cc";
          case "osoby w wieku 18-64 lata": return "#ff66cc";
          case "osoby w wieku 65 lat i więcej": return "#ff66cc";

          //"wg wykształcenia"
          case "niższe": return "#ff66cc";
          case "średnie": return "#ff66cc";
          case "wyższe": return "#ff66cc";

          //wg poziomu wykształcenia głowy gospodarstwa domowego"
          case "co najwyżej gimnazjalne": return isDot ? "ff33cc" : "#ff66cc";
          case "zasadnicze zawodowe": return isDot ? "ff33cc" : "#ff66cc";
          //case "średnie"
          //case "wyższe"

          //"wg stanu zawodowego osób"
          case "aktywni zawodowo": return isDot ? "ff33cc" : "#ff66cc";
          case "pracujący": return isDot ? "ff33cc" : "#ff66cc";
          case "bezrobotni": return isDot ? "ff33cc" : "#ff66cc";
          case "bierni zawodowo": return isDot ? "ff33cc" : "#ff66cc";

          //"wg grup społ.-ekonom. gospodarstw domowych"
          case "pracownicy": return isDot ? "ff33cc" : "#ff66cc";
          case "rolnicy": return isDot ? "ff33cc" : "#ff66cc";
          case "pracujący na własny rachunek": return isDot ? "ff33cc" : "#ff66cc";
          case "emeryci": return isDot ? "ff33cc" : "#ff66cc";
          case "renciści": return isDot ? "ff33cc" : "#ff66cc";
          */
        }

        return colors[this.chartDataProcessed.datasets.length % colors.length];
      }
    },
	
	  mounted() {
      console.log('chartcontainer mounted (data not in props yet btw)');
	  },

    updated() {
      if(!this.chartData){
        console.log("no data passed - returning");
        return;
      }
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

      this.chartDataProcessed.datasets = [];
      datasetsData.forEach(dataset => {
        console.log(dataset);
        var newDataset = {
          label: dataset[0],
          data: Object.values(dataset[1]),
          backgroundColor: this.calculateDataColor(dataset[0], true),
          borderColor: this.calculateDataColor(dataset[0], false),
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