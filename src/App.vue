<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <SelectionForm @updateChart="updateChart"/>
    </v-app-bar>

    <v-main>
      <ChartContainer :metadata="chartMetadata" :chartData="chartData"/>
    </v-main>
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
  }),

  methods: {
    updateChart(metadata, data) {
      console.log('App.updateChart()');
      this.chartMetadata = metadata;
      this.chartData = data;
    }
  }
};
</script>
