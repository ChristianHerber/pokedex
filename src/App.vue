<template>
  <div id="app" class="section has-background-light">
    <div class="container">
      <div class="columns is-multiline">

        <div class="column is-6 is-offset-3">
          <img src="./assets/pokemon-logo.png" alt="" class="">
        </div>

        <div class="column is-12">
          <div class="box column is-6 is-offset-3">
            <div class="field">
              <label for="" class="label">Pokédex</label>
              <div class="control">
                <input type="text" placeholder="Pesquisar Pokémon" class="input" v-model="busca">
              </div>
            </div>
            <button class="button is-fullwidth is-danger" @click="buscar" id="btnBuscar">Buscar</button>
          </div>
        </div>


        <div class="column is-3" v-for="(poke, index) in filteredPokemons" :key="poke.name">
          <Pokemon :name="poke.name" :url="poke.url" :num="index+1" />
        </div>

      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios'
import Pokemon from './components/Pokemon'

export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: '',
    }
  },
  created: function() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      console.log("Pegou os Pokemos")
      this.pokemons = res.data.results
      this.filteredPokemons = res.data.results
    })
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons
      if( this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
      console.log(this.filteredPokemons)
    }
  },
  computed: {
  /*  resultadoBusca: function(){
      if( this.busca == '' || this.busca == ' '){
        return this.pokemons
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    } */
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
}
#btnBuscar {
  margin-top: 5px;
}
</style>
