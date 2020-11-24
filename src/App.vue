<template>
  <div id="app">
    <h1 class="is-size-2">Pokedex</h1>
    <div class="column is-half is-offset-one-quarter">
      <input
        type="text"
        placeholder="Search a Pokemon"
        v-model="search"
        class="input is-rounded"
      />
      <button class="button is-fullwidth is-success" style="margin-top: 1%">
        Search
      </button>
      <div v-for="(pokemon, index) in searchResult" :key="pokemon.url">
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      search: "",
    };
  },
  created: function() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=152&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  computed: {
    searchResult: function() {
      if (this.search.trim().length == 0) {
        return this.pokemons;
      } else {
        return this.pokemons.filter(
          (pokemon) => pokemon.name == this.search.trim().toLowerCase()
        );
      }
    },
  },
};
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
