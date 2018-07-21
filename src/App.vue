<template>
  <div id="app">
  <vue-simple-suggest
    v-model="select"
    :list="simpleSuggestionList"
    :filter-by-query="true"
    @input="onChange">

<!-- Filter by input text to only show the matching results -->
  </vue-simple-suggest>
            <div v-if="loading">
          <img src="../src/assets/loading_icon.gif"/>
        </div> 
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
      select: '',
      poke: [],
      loading : false,
    }
  },
    created() {
     this.loading = false;
      this.getPokeNames();
  },
  methods:{


         simpleSuggestionList() {

          //this is to filter by the first letter the pokemon name starts with
         var pokeList = this.poke;
         var returnArr = [];
         var firstValue = this.select
         var firstLetter = firstValue.charAt(0);

         for(var i = 0; i<pokeList.length; i++) {
         //console.log(pokeList[i].startsWith(firstLetter.toUpperCase()))
          if(pokeList[i].startsWith(firstLetter.toUpperCase()) == true) {
          returnArr.push(pokeList[i])
          }
         }
        return returnArr

      },

      onChange(){

        console.log(this.select)
        var selected = this.select
        var allPoke = this.poke
         this.loadWeather(selected, allPoke);
      },


    loadWeather(selected, allPoke) {
  
      var valid = false;
      for(var i=0; i<allPoke.length; i++) {
        if(selected == allPoke[i]) {
          valid = true;
        }
      }

      if(valid){
    this.loading = true;
        
       axios.get('http://pokeapi.salestock.net/api/v2/pokemon/'+selected.toLowerCase())

      .then(res => {
      this.loading = false;
          var data = res.data;
     
      var results = [];
      console.log(data)
        this.results = results;
        
      })
      .catch(e => {
        this.loading = false;

        console.error(e);
      });
      }

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


  .vue-simple-suggest designed{
    margin-top: 50px;

}

</style>
