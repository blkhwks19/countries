<template>
  <v-container fluid>

    <v-row>
      <v-col cols="4">
        <v-text-field
          v-model="query"
          outlined
          clearable
          label="Search for a country..."
          prepend-inner-icon="mdi-magnify"
        ></v-text-field>
      </v-col>
      <v-spacer></v-spacer>
      <v-col cols="4">
        <v-select
          v-model="region"
          outlined
          label="Filter by Region"
          :items="getRegions()"
        ></v-select>
      </v-col>
    </v-row>


    <v-row v-if="countries.length === 0">
      <v-col
        v-for="i in 12"
        :key="i"
        cols="3"
      >
        <v-skeleton-loader
          type="card, paragraph"
        ></v-skeleton-loader>
      </v-col>
    </v-row>


    <v-row v-else>
      <v-col
        v-for="(country, index) in filteredCountries()"
        :key="index"
        cols="3"
      >

        <v-card @click="selectCountry(country)" style="height:100%;">
          <v-img
            :src="country.flag"
            style="border-bottom: 1px solid rgba(0, 0, 0, 0.15);"
          ></v-img>
          <v-card-title>{{ country.name }}</v-card-title>
          <v-card-text>
            <div><strong>Population:</strong> {{ country.population.toLocaleString() }}</div>
            <div><strong>Region:</strong> {{ country.region }}</div>
            <div><strong>Capital:</strong> {{ country.capital }}</div>
          </v-card-text>
        </v-card>

      </v-col>
    </v-row>

  </v-container>
</template>

<script>
import _ from 'lodash';

export default {
  name: 'Home',

  props: ['countries'],

  data() {
    return {
      region: 'All',
      query: '',
    }
  },

  methods: {
    getRegions() {
      let regions = this.countries.map(country => country.region);
      regions = _.uniq(regions).sort();
      regions[0] = 'All';
      return regions;
    },

    filteredCountries() {
      this.query = (this.query === null) ? '' : this.query;

      const filteredBySearch = this.countries.filter(country => 
        country.name.toLowerCase().includes(this.query.toLowerCase())
      )
      
      return (this.region === 'All') 
      ? filteredBySearch 
      : filteredBySearch.filter(country => country.region === this.region);
    },

    selectCountry(country) {
      this.$emit('country-selected', country);
    }
  }
}
</script>