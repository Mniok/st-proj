<template>
  <span>
    search form goes here
  </span>
</template>

<script>
	import axios from 'axios';

  export default {
    name: 'SelectionForm',

    emits: ['updateChart'],

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
		console.log('selection form mounted');
			
		axios.get('https://api.github.com/repos/statisticspoland/sdg-indicators-pl/git/blobs/9b66f28f696e971e7656f7852c61a9f5feaa748c')
			.then((response) => {
				var decodedData = decodeURIComponent(escape(window.atob( response.data.content )));
				var dataObjectTree = JSON.parse(decodedData);
        console.log('Pobierz wskaźniki krajowe dotyczące miasta i wsi - zamieszkanie_k.json - https://api.github.com/repos/statisticspoland/sdg-indicators-pl/git/blobs/9b66f28f696e971e7656f7852c61a9f5feaa748c');
				console.log(dataObjectTree);

        // retrieve metadata and data from api response to pass to chart
        var indicatorsData = Object.values(dataObjectTree)[0]; //"zamieszkanie_k"."0"
        var indicatorsDataArray = indicatorsData.map(indicator => Object.values(Object.values(indicator)))[0];
          var indicatorData = indicatorsDataArray[3]; //."1-1-a"    // !!!!!!!!!! replace with selects
        var chartMetadata = indicatorData.metadane[0]; // .metadane."0" -> {"nazwa", "definicja", "jednostka", ...}
        var chartDataGroups = indicatorData.dane[0]; //.dane."0" -> { "miasto/wieś": [...], "wg płci": [...] }
        chartDataGroups = Object.entries(chartDataGroups); // -> [["miasto/wieś", [values]], ["wg plci", [...]]]
          var chartData = chartDataGroups[0][1][0] //group0("miasto/wieś").values."0" -> array ["miasto", "wieś"]  //!!!!!!!!!! replace with select

        for (const key in chartData) {
          chartData[key] = chartData[key][0]; // eg. "kobiety"."0".[array] -> "kobiety".[array]
        }

        console.log('emit:');
        this.$emit('updateChart', chartMetadata, chartData);
			})
			.catch((error) => {
				console.log('axios error:');
				console.log(error);
			})
			/*.finally(() => {
				//always executed
			});*/
	}
  }
</script>
