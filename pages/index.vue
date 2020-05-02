<template>
  <v-layout>
    <v-flex>
      <v-row>
        <p v-if="$fetchState.pending">Fetching data...</p>
        <p v-else-if="$fetchState.error">Error while fetching data: {{ $fetchState.error.message }}</p>
        <v-col v-else
          v-for="item of dataCount"
          :key="item.title"
          cols="12"
          sm="4"
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
            >
              <template v-slot:item.country="{ item }">
                <v-chip :to="item.country.toLowerCase().trim()" dark>{{ item.country }}</v-chip>
              </template>
            </v-data-table>
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
          text: 'Country',
          align: 'start',
          value: 'country',
        },
        { text: 'Total Cases', value: 'totalCases' },
        { text: 'New Cases', value: 'newCases' },
        { text: 'Total Deaths', value: 'totalDeaths' },
        { text: 'New Deaths', value: 'newDeaths' },
        { text: 'Total Recovered', value: 'totalRecovered' },
      ],
      dense: false,
      fixedHeader: true,
      height: 500,
    }
  },
  async fetch () {
    this.dataStore = await this.$http.$get('http://localhost:3001/covid-data')
  },
  computed: {
    dataCount() {
      return this.dataStore[0];
    }

  }

}
</script>
