<template>
  <div id="app">
     <div class="row">
       <div class="col s12 m6">
  <vue-simple-suggest
    v-model="select"
    :list="simpleSuggestionList"
    :filter-by-query="true"
    @input="onChange">

<!-- Filter by input text to only show the matching results -->
  </vue-simple-suggest>
  </div>
  </div>
            <div v-if="loading">
          <img src="../src/assets/loading_icon.gif"/>
        </div> 

           <div v-if="showPoke">
  <div class="cardWrapper">
 <div class="row">
    <div class="col s12 m6">
      <div class="card blue-grey darken-1">
        <div class="card-content white-text">
          <span class="card-title">{{results.name}}</span>
          <p>I am a very simple card. I am good at containing small bits of information.
          I am convenient because I require little markup to use effectively.
          <!-- <img src="https://img.pokemondb.net/artwork/pikachu.jpg"> -->
        <img v-bind:src="'https://img.pokemondb.net/artwork/'+results.name + '.jpg'">

        </p>
        </div>
        <div class="card-action">
          <a href="#">This is a link</a>
          <a href="#">This is a link</a>
        </div>
      </div>
    </div>
  </div>


  </div>

        </div>
    </div>
</template>




<script>

import axios from 'axios'
import moment from 'moment'
import VueSimpleSuggest from 'vue-simple-suggest'
  import 'vue-simple-suggest/dist/styles.css' // Using a css-loader
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
      showPoke : false
    }
  },
    created() {
     this.loading = false;
     this.showPoke = false;
      this.getPokeNames();

  },
  methods:{


         simpleSuggestionList() {

     this.showPoke = false;
          //this is to filter by the firstletter the pokemon name starts with
         var pokeList = this.poke;
         var returnArr = [];
         var firstValue = this.select
         var firstLetter = firstValue.charAt(0);
         //var check = this
console.log(this)

         for(var i = 0; i<pokeList.length; i++) {
         //console.log(pokeList[i].startsWith(firstLetter.toUpperCase()))
          if(pokeList[i].startsWith(firstLetter.toUpperCase()) == true) {
          returnArr.push(pokeList[i])
          }
         }
         console.log(returnArr)
        return this.poke

      },

      onChange(){
     this.showPoke = false;
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
      this.showPoke = true;
      var data = res.data;
     
      var results = [];
      console.log(data)
        this.results = data;
        
      })
      .catch(e => {
        this.loading = false;
        this.showPoke = false;

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

.button
{
  width: 150px;
  padding: 10px;
  background-color: #FF8C00;
  box-shadow: -8px 8px 10px 3px rgba(0,0,0,0.2);
    font-weight:bold;
  text-decoration:none;
}
.cover{
    position:fixed;
    top:0;
    left:0;
    background:rgba(0,0,0,0.6);
    z-index:5;
    width:100%;
    height:100%;
    display:none;
}
.loginScreen
{
    height:380px;
    width:340px;
    margin:0 auto;
    position:relative;
    z-index:10;
    display:none;
  border:5px solid #cccccc;
  border-radius:10px;
}
.loginScreen:target, .loginScreen:target + .cover{
    display:block;
    opacity:2;
}
.cancel
{
    display:block;
    position:absolute;
    top:3px;
    right:2px;
    background:rgb(245,245,245);
    color:black;
    height:30px;
    width:35px;
    font-size:30px;
    text-decoration:none;
    text-align:center;
    font-weight:bold;
}

</style>
