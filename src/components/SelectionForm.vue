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
        console.log('----------------- indicData metadata chartdata :')

        //console.log(dataObjectTree.zamieszkanie_k); works but don't want to hardcode this
        //console.log(dataObjectTree[0][0]); error - tries to access ."0"
        //console.log(dataObjectTree[Object.keys(dataObjectTree)]); // this works fine as long as obj kas 1 key
        //console.log(Object.values(dataObjectTree)); // this works fine always
        var indicatorsData = Object.values(dataObjectTree)[0]; //"zamieszkanie_k"."0"
        var indicatorsDataArray = indicatorsData.map(indicator => Object.values(Object.values(indicator)))[0];
        console.log(Object.values(indicatorsDataArray));
          var indicatorData = indicatorsDataArray[3]; //."1-1-a"    // !!!!!!!!!! replace with selects
        console.log(indicatorData);
        var chartMetadata = indicatorData.metadane[0]; // .metadane."0" -> {"nazwa", "definicja", "jednostka", ...}
        console.log(chartMetadata);
        var chartDataGroups = indicatorData.dane[0]; //.dane."0" -> { "miasto/wieś": [...], "wg płci": [...] }
        chartDataGroups = Object.entries(chartDataGroups); // -> [["miasto/wieś", [values]], ["wg plci", [...]]]
        console.log(chartDataGroups);
        var chartData = chartDataGroups[0][1][0] //group0("miasto/wieś").values."0" -> array ["miasto", "wieś"]  //!!!!!!!!!! replace with select
        console.log(chartData);

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
