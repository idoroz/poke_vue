<template>
  <div id="app">
  <vue-simple-suggest
    v-model="chosen"
    :list="simpleSuggestionList"
    :filter-by-query="true">
<!-- Filter by input text to only show the matching results -->
  </vue-simple-suggest>
 <nav>
    <div class="nav-wrapper">
      <form>
        <div class="input-field">
          <input id="search" type="search" required>
          <label class="label-icon" for="search"><i class="material-icons">search</i></label>
          <i class="material-icons">close</i>
        </div>
      </form>
    </div>
  </nav>
           
  </div>
</template>




<script>

import axios from 'axios'
import moment from 'moment'
import VueSimpleSuggest from 'vue-simple-suggest'
import 'vue-simple-suggest/dist/styles.css'
import pokeNAMES from './assets/pokemon_names.js'


export default {
      components: {
      VueSimpleSuggest
    },
  data() {
    return {
      title: 'hello',
      searching: false,
      cod: '',
      lat: {},
      lon: {},
      results : [],
      chosen: '',
      poke: []
    }
  },
    created() {

      this.loadWeather();
      this.getPokeNames();
  },
  methods:{


         simpleSuggestionList() {
        return this.poke

      },

    loadWeather() {

       axios.get(`http://pokeapi.salestock.net/api/v2/pokemon/butterfree`)

      .then(res => {

          var data = res.data;
     
      var results = [];
      console.log(data)
        this.results = results;
        
      })
      .catch(e => {
        console.error(e);
      });
        
    },
    getPokeNames() {

      var check = pokeNAMES.toString();
      var res = check.split(",");
      var pokeNameArr = [];

      for(var i = 0; i<res.length; i++) {
        pokeNameArr.push(res[i])
      }

       this.poke = pokeNameArr
    },

  },
}

</script>

<style>

</style>
