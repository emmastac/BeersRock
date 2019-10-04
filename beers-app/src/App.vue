<template>
<v-app light>
  <!--The SideMenu Component go here-->
  <v-toolbar fixed app light clipped-left color="primary" class="elevation-2">
    <v-toolbar-side-icon @click="drawer = !drawer"  class="white--text"></v-toolbar-side-icon>
    <v-toolbar-title class="white--text">Beers App</v-toolbar-title>
  </v-toolbar>

  <v-content>
    <v-container fluid>
      <v-data-table
        :headers="headers"
        :items="beers"
        hide-default-footer
      ></v-data-table>
      <div class="text-center">
      <v-pagination
        v-model="page"
        :length="8"
        @input="fetchPage"
      ></v-pagination>
    </div>
    </v-container>
   </v-content>
   <v-footer class="secondary" app>
      <v-layout row wrap align-center>
        <v-flex xs12>
          <div class="white--text ml-3">
            Made with
            <v-icon class="red--text">favorite</v-icon>
            by <a class="white--text" href="https://vuetifyjs.com" target="_blank">Vuetify</a>
            and <a class="white--text" href="https://github.com/rachidsakara"
target="_blank">Rachid Sakara</a>
          </div>
        </v-flex>
      </v-layout>
    </v-footer>
  </v-app>


</template>

<script>

import axios from 'axios' // importing the axios (a HTTP library) to connects the app with the punk API

export default {

  data() {
    return {
      options: {},
      drawer: false, // false = Vuetify automatically "do the right thing" to show/hide the drawer
      beers: [],
      headers: [
        {
          text: 'Name',
          align: 'left',
          sortable: false,
          value: 'name',
        },
        { text: 'Title', value: 'tagline' },
        { text: 'Description', value: 'description' }
      ],
      page: 1,
      errors: []
    }
  },
  watch: {
    pagination: {
      handler () {
        fetchPage();
      },
      deep: true
  }
},
  methods: {
    fetchPage() {
      //alert('sth');
       axios.get('https://api.punkapi.com/v2/beers?page='+this.page+'&malt=extra_pale')
        .then(response => {
          this.beers = response.data
        })
        .catch(e => {
          this.errors.push(e)
        })
    }
  },

  created () {
    this.fetchPage()
  },


  }
</script>
