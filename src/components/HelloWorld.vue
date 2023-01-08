<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Vuetify
        </h1>

        <p class="subheading font-weight-regular">
          For help and collaboration with other Vuetify developers,
          <br>please join our online
          <a
            href="https://community.vuetifyjs.com"
            target="_blank"
          >Discord Community</a>
        </p>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          What's next?
        </h2>

        <v-row justify="center">
          <a
            v-for="(next, i) in whatsNext"
            :key="i"
            :href="next.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ next.text }}
          </a>
        </v-row>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Important Links
        </h2>

        <v-row justify="center">
          <a
            v-for="(link, i) in importantLinks"
            :key="i"
            :href="link.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ link.text }}
          </a>
        </v-row>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Ecosystem
        </h2>

        <v-row justify="center">
          <a
            v-for="(eco, i) in ecosystem"
            :key="i"
            :href="eco.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ eco.text }}
          </a>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
	import axios from 'axios';

  export default {
    name: 'HelloWorld',

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
      importantLinks: [
        {
          text: 'Documentation',
          href: 'https://vuetifyjs.com',
        },
        {
          text: 'Chat',
          href: 'https://community.vuetifyjs.com',
        },
        {
          text: 'Made with Vuetify',
          href: 'https://madewithvuejs.com/vuetify',
        },
        {
          text: 'Twitter',
          href: 'https://twitter.com/vuetifyjs',
        },
        {
          text: 'Articles',
          href: 'https://medium.com/vuetify',
        },
      ],
      whatsNext: [
        {
          text: 'Explore components',
          href: 'https://vuetifyjs.com/components/api-explorer',
        },
        {
          text: 'Select a layout',
          href: 'https://vuetifyjs.com/getting-started/pre-made-layouts',
        },
        {
          text: 'Frequently Asked Questions',
          href: 'https://vuetifyjs.com/getting-started/frequently-asked-questions',
        },
      ],
    }),
	
	mounted() {
		console.log('mounted');
		
		/*axios.get('https://api.github.com/repos/statisticspoland/sdg-indicators-pl/contents/api/v1/krajowe/zamieszkanie_k.json')
			.then((response) => {
				console.log('1. link .json');
				console.log('response:');
				console.log(response);
			})
			.catch((error) => {
				console.log('error:');
				console.log(error);
			})
			.finally(() => {
				//always executed
				console.log('finally');
			});
		*/
			
		axios.get('https://api.github.com/repos/statisticspoland/sdg-indicators-pl/git/blobs/9b66f28f696e971e7656f7852c61a9f5feaa748c')
			.then((response) => {
				/*console.log('2. git_url');
				console.log('response:');
				console.log(response);
				
				console.log('data.content:')
				console.log(response.data.content);
				
				console.log('decoded from base64:');
				console.log(btoa(response.data.content));
				console.log(atob(response.data.content));
				
				console.log('decoeURI:');
				console.log(decodeURIComponent(escape(window.atob( response.data.content )))); // z https://stackoverflow.com/questions/30106476/using-javascripts-atob-to-decode-base64-doesnt-properly-decode-utf-8-strings
				*/
				
				var decodedData = decodeURIComponent(escape(window.atob( response.data.content )));
				var dataObjectTree = JSON.parse(decodedData);
        console.log('Pobierz wskaźniki krajowe dotyczące miasta i wsi - zamieszkanie_k.json - https://api.github.com/repos/statisticspoland/sdg-indicators-pl/git/blobs/9b66f28f696e971e7656f7852c61a9f5feaa748c');
				console.log(dataObjectTree);
			})
			.catch((error) => {
				console.log('error:');
				console.log(error);
			})
			.finally(() => {
				//always executed
				console.log('_______');
			});


      // // // other endpoints
      axios.get('https://api.github.com/repos/statisticspoland/sdg-indicators-pl/contents/api/v1/globalne/plec.json')
			.then((response) => {
				var decodedData = decodeURIComponent(escape(window.atob( response.data.content )));
				var dataObjectTree = JSON.parse(decodedData);
        console.log('globalne i krajowe dotyczące płci - https://api.github.com/repos/statisticspoland/sdg-indicators-pl/contents/api/v1/globalne/plec.json');
				console.log(dataObjectTree);
			})
			.catch((error) => {
				console.log('error:');  //apparrently endpoint not found - 404
				console.log(error);
			})


      axios.get('https://api.github.com/repos/statisticspoland/sdg-indicators-pl/contents/api/v1/zamieszkanie.json')
			.then((response) => {
				var decodedData = decodeURIComponent(escape(window.atob( response.data.content )));
				var dataObjectTree = JSON.parse(decodedData);
        console.log('Pobierz wskaźniki krajowe dotyczące miasta i wsi - https://api.github.com/repos/statisticspoland/sdg-indicators-pl/contents/api/v1/zamieszkanie.json');
				console.log(dataObjectTree);
			})
			.catch((error) => {
				console.log('error:');
				console.log(error);
			})

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
	}
  }
</script>
