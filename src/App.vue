<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-icon x-large>mdi-earth</v-icon>
      <div class="text-h5 ml-2">Countries of the World</div>

      <v-spacer></v-spacer>

      <v-btn
        large
        href="https://github.com/blkhwks19/countries" 
        target="_blank"
      >
        <v-icon class="mr-2">mdi-github</v-icon>
        Github
      </v-btn>
    </v-app-bar>

    <v-main>
      
      <Home v-if="!detail" :countries="countries" @country-selected="countrySelected" />
      <Detail v-else :countries="countries" :country="country" @back="back" />

    </v-main>
  </v-app>
</template>

<script>
import Home from '@/components/Home.vue';
import Detail from '@/components/Detail.vue';

export default {
  name: 'App',

  components: {
    Home,
    Detail,
  },

  data: () => ({
    countries: [],
    country: null,
    detail: false,
  }),

  created() {
    this.getCountries();
  },

  methods: {
    getCountries() {
      fetch(`https://restcountries.eu/rest/v2/all`)
        .then(res => res.json())
        .then(data => this.countries = data)
        .catch(err => {
          console.log('ERROR GETTING COUNTRIES');
          console.log(err.message);
        });
    },

    countrySelected(country) {
      this.country = country;
      this.detail = true;
    },

    back() {
      this.detail = false;
    }
  }
};
</script>
