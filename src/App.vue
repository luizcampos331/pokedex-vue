<template>
  <div id="app">
    <div class="cards">
      <img class="logo" src="./assets/logo.png" alt="logo pokemon">

      <input type="text" placeholder="Buscar pokemon" v-model="search">

      <div class="card" v-for="(pokemon, index) in searchResult" :key="pokemon.url">
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  components: {
    Pokemon
  },
  data() {
    return {
      pokemons: [],
      search: '',
    }
  },
  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(response => {
      this.pokemons = response.data.results;
    }).catch(error => {
      console.log(error);
    });
  },
  computed: {
    searchResult: function() {
      if(this.search === '' || this.search === ' ') {
        return this.pokemons;
      }

      return this.pokemons.filter(pokemon => pokemon.name.includes(this.search));
    }
  }
}
</script>

<style>
  #app {
    text-align: center;
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  
  .logo {
    width: 100%;
    margin: 40px 0;
  }

  input {
    width: 100%;
    padding: 10px 15px;
    border-radius: 20px;
    border: 1px solid transparent;
    border-color: #bbbbbb;
  }

  input:hover {
    transition: 200ms;
    border-color: #2a75bb;
  }

  input:focus {
    border-color: #2a75bb;
  }

  .cards {
    width: 20%;
    min-width: 150px;
  }

  .card {
    border-radius: 5px;
    padding: 8px;
    border-width: 1px;
    box-shadow: 3px 3px 5px 2px rgba(0, 0, 0, 0.4);
    color: #4a4a4a;
    width: 100%;
    margin-top: 20px;
  }
</style>
