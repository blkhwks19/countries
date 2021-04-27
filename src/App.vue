<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="text-h5">Where in the world?</div>

      <v-spacer></v-spacer>

      <v-btn
        text
      >
        <v-icon>mdi-weather-night</v-icon>
        <span class="ml-2">Dark Mode</span>
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
