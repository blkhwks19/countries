<template>
  <v-container>

    <v-row class="my-3">
      <v-btn @click="back()">
        <v-icon>mdi-keyboard-backspace</v-icon>
        Back
      </v-btn>
    </v-row>

    <v-row align="center">
      <v-col class="pl-0" cols="6">
        <v-img
          :src="country.flag"
          style="border: 1px solid rgba(0, 0, 0, 0.15);"
        ></v-img>
      </v-col>
      <v-col cols="6">
        
        <v-row>
          <v-card-title class="text-h4 font-weight-bold pl-3">
            {{ country.name }}
            <v-tooltip top open-delay="100">
              <template v-slot:activator="{ on }">
                <v-btn 
                  icon 
                  
                  :ripple="false"
                  color="primary" 
                  class="ml-3" 
                  :href="`https://en.wikipedia.org/wiki/${country.name}`"
                  target="_blank"
                  v-on="on"
                >
                  <v-icon>mdi-wikipedia</v-icon>
                </v-btn>
              </template>
              <span>Wikipedia</span>
            </v-tooltip>
            <v-tooltip top open-delay="100">
              <template v-slot:activator="{ on }">
                <v-btn 
                  icon 
                  
                  :ripple="false"
                  color="primary" 
                  :href="`https://www.google.com/maps/place/${country.name}`"
                  target="_blank"
                  v-on="on"
                >
                  <v-icon>mdi-google-maps</v-icon>
                </v-btn>
              </template>
              <span>Google Maps</span>
            </v-tooltip>
          </v-card-title>
        </v-row>
        
        <v-row>
          <v-col class="py-1">
            <strong>Native Name:</strong> {{ country.nativeName }}
          </v-col>
          <v-col class="py-1">
            <strong>Top Level Domain:</strong> {{ country.topLevelDomain[0] }}
          </v-col>
        </v-row>

        <v-row>
          <v-col class="py-1">
            <strong>Population:</strong> {{ country.population.toLocaleString() }}
          </v-col>
          <v-col class="py-1">
            <strong>Currencies:</strong> {{ getCurrencies(country.currencies) }}
          </v-col>
        </v-row>

        <v-row>
          <v-col class="py-1">
            <strong>Region:</strong> {{ country.region }}
          </v-col>
          <v-col class="py-1">
            <strong>Languages:</strong> {{ getLanguages(country.languages) }}
          </v-col>
        </v-row>

        <v-row>
          <v-col class="py-1">
            <strong>Sub Region:</strong> {{ country.subregion }}
          </v-col>
        </v-row>

        <v-row>
          <v-col class="py-1">
            <strong>Capital:</strong> {{ country.capital }}
          </v-col>
        </v-row>

        <v-row>
          <v-col class="pt-5">
            <div>
              <strong>Border Countries:</strong>
              <div v-if="getBorders(country.borders).length === 0">None</div>
              <v-chip-group v-else column>
                <v-chip
                  v-for="(item, index) in getBorders(country.borders)"
                  :key="index"
                  :ripple="false"
                  label
                  color="primary"
                  style="cursor:default;"
                >
                  {{ item.name }}
                </v-chip>
              </v-chip-group>
            </div>
          </v-col>
        </v-row>

      </v-col>
    </v-row>
      
  </v-container>
</template>

<script>
export default {
  name: 'Detail',

  props: ['countries', 'country'],

  methods: {
    back() {
      this.$emit('back');
    },

    getCurrencies(currs) {
      return currs.map(curr => curr.name).join(', ');
    },

    getLanguages(langs) {
      return langs.map(lang => lang.name).join(', ');
    },

    getBorders(codes) {
      const borders = [];
      for (let i = 0; i < codes.length; i++) {
        for (let j = 0; j < this.countries.length; j++) {
          if (codes[i] === this.countries[j].alpha3Code) {
            borders.push(this.countries[j]);
            break;
          }
        }
      }

      return borders;
    }
  }
}
</script>