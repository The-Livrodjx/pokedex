<template>
  <div id="app">

    <div class="column is-full">
      <nav class="navbar" role="navigation" aria-label="main navigation">
        <div class="navbar-brand">
          <a class="navbar-item" href="https://bulma.io">
            <img src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/i/87044f58-c765-43c5-bc51-8613e3ac7ab1/ddew4m7-c69a2c41-518f-48ca-ba35-8ab1895464e0.png" width="112" height="28">
          </a>

          <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample" onclick="document.querySelector('.navbar-menu').classList.toggle('is-active');">
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>

        <div id="navbarBasicExample" class="navbar-menu">
          <div class="navbar-start">
            
          </div>

          
          <div class="navbar-end">
            <div class="navbar-item">
              <input class="input is-link" v-model="busca" type="text" id="buscarPoke" placeholder="Buscar Pokemon pelo nome">
              <button class="button is-primary is-outlined" @click="buscar">Buscar</button>
            </div>
          </div>
        </div>
      </nav>
    </div>
    

    <div class="column is-half is-offset-one-quarter has-text-centered">
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import { api } from "./services/api";
import Pokemon from "./components/Pokemon";

export default {

  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ""
    };
  },

  created: function () {
    // When the page is loaded
    api.get("/pokemon?limit=151&offset=0").then((res) => {
      console.log(res.data.results);
      this.pokemons = res.data.results; // Insert into array pokemons
      this.filteredPokemons = res.data.results; // Insert into array pokemons
    });
  },
  methods: {

    buscar: function() {
      this.filteredPokemons = this.pokemons

      if(this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons
      }
      else {
        this.busca = this.busca.toLowerCase()
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    },

    showHamburguer: function() {
      var burger = document.querySelector('.navbar-burger');
      var menu = document.querySelector('.navbar-menu');
      burger.addEventListener('click', function() {
          burger.classList.toggle('is-active');
          menu.classList.toggle('is-active');
      });
    }
  },
  // computed: {
  //   resultadoBusca: function() {

  //     if(this.busca == "" || this.busca == " "){
  //       return this.pokemons

  //     }else {
  //       return this.pokemons.filter(pokemon => pokemon.name == this.busca)
  //     }
  //   }
  // }
};
</script>

<style>
#buscarPoke {
  width: 250px;
}
</style>
