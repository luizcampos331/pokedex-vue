<template>
  <div id="app">
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      pokemons: [],
    }
  },
  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(response => {
      this.pokemons = response.data.results;
    }).catch(error => {
      console.log(error);
    });
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
