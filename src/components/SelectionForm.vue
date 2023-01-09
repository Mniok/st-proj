<template>
  <span>
    <v-form>
      <v-row>
        <v-select
          label="Wskaźnik"
          v-model="selectedIndicatorKey"
          :items="indicatorSelectItems"
          outlined
        />
        <v-select
          label="Wykres"
          v-model="selectedChartDatasetKey"
          :items="chartDatasetSelectItems"
          @change="updateChart"
          outlined
        />
      </v-row>
    </v-form>
  </span>
</template>

<script>
	import axios from 'axios';

  export default {
    name: 'SelectionForm',

    emits: ['updateChart'],

    data: () => ({
      dataFromAPI: {},
      selectedIndicatorKey: "",
      selectedChartDatasetKey: "",
    }),

    methods: {
      readFromAPI() {
        axios.get('https://api.github.com/repos/statisticspoland/sdg-indicators-pl/git/blobs/9b66f28f696e971e7656f7852c61a9f5feaa748c')
        .then((response) => {
          var decodedData = decodeURIComponent(escape(window.atob( response.data.content )));
          this.dataFromAPI = JSON.parse(decodedData);
          console.log(this.dataFromAPI);
        })
        .catch((error) => {
          console.log('Wystąpił błąd:');
          console.log(error);
        })

      },
      selectIndicactor(indicatorKey) {

      },
      selectChart(chartKey) {

      },
      updateChart(){
        console.log("SELECTED " + this.selectedIndicatorKey + " AND " + this.selectedChartDatasetKey);

        // retrieve metadata and data from api response to pass to chart
        var indicatorsData = Object.values(this.dataFromAPI)[0][0]; //["zamieszkanie_k"."0"]."0"
        //console.log(indicatorsData);
        //console.log(this.selectedIndicatorKey);
        //console.log(indicatorsData[this.selectedIndicatorKey]);
        //var indicatorsDataArray = indicatorsData.map(indicator => Object.values(Object.values(indicator)))[0];
        //var indicatorData = indicatorsDataArray[3]; //."1-1-a"
        var indicatorData = indicatorsData[this.selectedIndicatorKey];
        var chartMetadata = indicatorData.metadane[0]; // .metadane."0" -> {"nazwa", "definicja", "jednostka", ...}
        var chartDataGroups = indicatorData.dane[0]; //.dane."0" -> { "miasto/wieś": [...], "wg płci": [...] }
        //chartDataGroups = Object.entries(chartDataGroups); // -> [["miasto/wieś", [values]], ["wg plci", [...]]]
        //console.log(chartDataGroups);
        var chartData = chartDataGroups[this.selectedChartDatasetKey][0] //group0("miasto/wieś").values."0" -> array ["miasto", "wieś"]

        for (const key in chartData) {
          chartData[key] = chartData[key][0]; // eg. "kobiety"."0".[array] -> "kobiety".[array]
        }

        //console.log('emit:');
        //console.log(chartMetadata);
        console.log(chartData);
        //console.log(this.selectedChartDatasetKey);
        this.$emit('updateChart', chartMetadata, chartData, this.selectedChartDatasetKey);
      }
    },

    computed: {
      indicatorSelectItems() {
        //console.log('INDICATOR SELECT ITEMS');
        //console.log(this.dataFromAPI);
        //console.log(Object.values(this.dataFromAPI));
        //console.log(Object.values(this.dataFromAPI)[0]);
        if (!Object.values(this.dataFromAPI)[0])
          return [];
        else {
          var indicatorsData = Object.values(this.dataFromAPI)[0][0]; //["zamieszkanie_k"."0"]."0" //Object.values returns array of 1 element
          //console.log(indicatorsData);
          var itemValues = Object.keys(indicatorsData);
          var itemTexts = Object.values(indicatorsData).map(
            value => value.metadane[0].nazwa
          );
          //console.log('itemvals, itemtexts');
          //console.log(itemValues);
          //console.log(itemTexts);

          var items = [];
          for (var i=0; i<itemValues.length; i++) {
            items.push({
              value: itemValues[i],
              text: itemTexts[i]
            });
          }

          console.log("indicatorSelect items");
          console.log(items);
          return items;
        }
      },
      chartDatasetSelectItems() {
        console.log('CHART DATASET SELECT ITEMS');
        console.log(this.selectedIndicatorKey);
        const up = this.selectedIndicatorKey;
        if (!Object.values(this.dataFromAPI)[0] || !this.selectedIndicatorKey)
          return [];
        else {
          // retrieve metadata and data from api response to pass to chart
          var indicatorData = Object.values(this.dataFromAPI)[0][0][this.selectedIndicatorKey]; //("zamieszkanie_k"."0")[0]]."1-1-a"
          console.log(indicatorData);
          var chartDataGroups = indicatorData.dane[0]; //.dane."0" -> { "miasto/wieś": [...], "wg płci": [...] }
          //chartDataGroups = Object.entries(chartDataGroups); // -> [["miasto/wieś", [values]], ["wg plci", [...]]]
          console.log('chart data groups')
          console.log(chartDataGroups);
          const items = Object.keys(chartDataGroups); //text and value are the same here
          console.log(items);
          return items;
        }
      },
    },
	
    mounted() {
      console.log('selection form mounted');
      this.readFromAPI();

    }
  }
</script>
