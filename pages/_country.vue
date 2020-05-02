<template>
  <v-layout>
    <v-flex>
      <v-row>
        <v-col cols="12">
          <v-card>
            <v-card-title>{{country.toUpperCase().trim()}}</v-card-title>
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <p v-if="$fetchState.pending">Fetching data...</p>
        <p v-else-if="$fetchState.error">Error while fetching data: {{ $fetchState.error.message }}</p>
        <v-col v-else
          v-for="item of dataCount"
          :key="item.title"
          cols="12"
          sm="3"
        >
          <v-card>
            <v-card-text class="text-center display-2">{{item.count}}</v-card-text>
            <v-card-text class="text-center title" >{{item.title}}</v-card-text>
          </v-card>
        </v-col>
        <v-col>
          <v-card>
            <v-card-title>
              <v-text-field
                v-model="search"
                label="Search"
                single-line
                hide-details
              ></v-text-field>
            </v-card-title>
            <v-data-table
              :headers="headers"
              :items="dataStore[1]"
              :search="search"
              :items-per-page="5"
              class="elevation-1"
            ></v-data-table>
          </v-card>
        </v-col>
      </v-row>
    </v-flex>
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data(){
    return {
      dataStore: [],
      search: '',
      headers: [
        {
          text: 'State',
          align: 'start',
          value: 'state',
        },
        { text: 'Total Cases', value: 'totalCases' },
        { text: 'Total Deaths', value: 'totalDeaths' },
        { text: 'Total Recovered', value: 'totalRecovered' },
      ],
      dense: false,
      fixedHeader: true,
      height: 500,
    }
  },
  async fetch () {
    this.dataStore = await this.$http.$get('http://localhost:3001/covid-data/'+this.country)
  },
  computed: {
    dataCount() {
      return this.dataStore[0];
    },
    country() {
      return this.$route.params.country
    }

  }

}
</script>
 