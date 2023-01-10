<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
      
    >
      <SelectionForm
        :endpoint="APIEndpointURL"
        @updateChart="updateChart"
      />
    </v-app-bar>

    <v-main>
      <ChartContainer
        :metadata="chartMetadata"
        :chartData="chartData"
        :selectedDatasetName="selectedDatasetName"
      />
    </v-main>
    <v-footer color="grey lighten-1" class="mt-0 mb-0 pt-0 pb-0">
      <v-container fluid class="mt-0 mb-0 pt-0 pb-0">
        <v-row align="center">
          <v-col class="d-flex justify-end" cols="12" sm="5">
            <v-img
              :src="require('./assets/apiLogo.png')"
              max-width="120"
            />
          </v-col>
          <v-col class="d-flex" cols="12" sm="7">
            <v-card-text>
              Serwis korzysta z danych udostępnianych przez API SDG GUS
            </v-card-text>
          </v-col>
        </v-row>
      </v-container>

    </v-footer>
  </v-app>
</template>

<script>
import ChartContainer from './components/ChartContainer';
import SelectionForm from './components/SelectionForm';

export default {
  name: 'App',

  components: {
    ChartContainer,
    SelectionForm,
  },

  data: () => ({
    chartMetadata: [],
    chartData: [],
    selectedDatasetName: "",
    APIEndpointURL: "https://api.github.com/repos/statisticspoland/sdg-indicators-pl/git/blobs/9b66f28f696e971e7656f7852c61a9f5feaa748c",
  }),

  methods: {
    updateChart(metadata, data, selectedDataset) {
      console.log('App.updateChart()');
      this.chartMetadata = metadata;
      this.chartData = data;
      this.selectedDatasetName = selectedDataset;
    }
  }
};
</script>
