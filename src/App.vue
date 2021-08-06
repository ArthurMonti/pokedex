<template>
  
  <div id="app">
    
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokeapi.png">
      <hr>
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text" name="" id="" placeholder="Buscar Pokemon pelo nome" v-model="busca" class="input is-rounded">
      <button class="button is-fullwidth is-success" id="buscaBtn" @>Buscar</button> <!-- colocar @click="buscar" se quiser utilizar o botão--> 
      <div v-for="(poke, index) in resultadoBusca" :key="poke.url"> <!-- colocar "in filteredPokemons" se quiser utilizar o botão--> 
        <Pokemon :name="poke.name" :url="poke.url" :num="index +1" />
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
      busca: '',
      filteredPokemons: []
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  methods:{
      buscar: function(){
        this.filteredPokemons = this.pokemons;
        if(this.busca == '' || this.busca ==' ')
        {
          this.filteredPokemons = this.pokemons;
        }else{
          this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.includes(this.busca));
        }
      }
  },
  computed: {
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' ')
      {
        return this.pokemons;
      }
      else{
        return this.pokemons.filter(pokemon => pokemon.name.includes(this.busca));
      }
      
    }
  }
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

#buscaBtn{
  margin-top: 2%
}
</style>
