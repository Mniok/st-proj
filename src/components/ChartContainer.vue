<template>
  <v-container>
    <br><br>
    <p>{{ metadata.name }}</p>
  </v-container>
</template>

<script>
	import axios from 'axios';

  export default {
    name: 'ChartContainer',

    props: ['metadata', 'chartData'],

    data: () => ({
      ecosystem: [
        {
          text: 'vuetify-loader',
          href: 'https://github.com/vuetifyjs/vuetify-loader',
        },
        {
          text: 'github',
          href: 'https://github.com/vuetifyjs/vuetify',
        },
        {
          text: 'awesome-vuetify',
          href: 'https://github.com/vuetifyjs/awesome-vuetify',
        },
      ],
    }),
	
	  mounted() {
      console.log('chartcontainer mounted');

      console.log(this.chartData);
	  },

    updated() {
      console.log('chartcontiner updated');
      console.log(this.metadata);
      console.log(this.chartData);
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