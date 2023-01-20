<template>
  <div class="body">
    <main class="main">
      <!-- imagem animada do pokémon retirada da api -->
    <img v-bind:src="pokemonImage" class="pokemon__image">
      
      <!-- id do pokémon (número do pokémon) -->
    <h1 class="pokemon__data">
      <span class="pokemon__number"
      v-text="pokemonID"
      ></span> -
      <!-- nome do pokémon  -->
      <span class="pokemon__name"
      v-text="pokemonName"></span>
    </h1>

    <!-- formulário de busca do pokémon, através do nome ou id do pokémon -->
    <!-- required para não ser permitido enviar formulário vazio -->
    <form class="form" @submit.prevent="searchPokemon">
      <input
        type="search"
        class="input__search"
        placeholder="Nome ou Número"
        v-model="searchValue"
        required
      />
    </form>

    <!-- botão de passar para o id anterior de pokemon -->
    <div class="buttons">
      <button class="button btn-prev"
      @click="btnPrev"
      >&lt; Prev</button>
    <!-- botão de passar para o próximo id de pokemon -->  
      <button class="button btn-next"
      @click="btnNext"
      >Next &gt;</button>
    </div>

    <!-- imagem de toda a pokedex -->
    <img src="./assets/pokedex.png" alt="pokedex" class="pokedex">
  </main>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      pokemonName: '',
      pokemonID: '',
      pokemonImage: '',
      searchValue: '',
    }
  },
  created() {
    // ao iniciar o site, sempre renderizar o primeiro pokemon
    axios.get(`https://pokeapi.co/api/v2/pokemon/1`)
    .then(response => {
      this.pokemonName = response.data.name;
      this.pokemonID = response.data.id;
      this.pokemonImage = response.data['sprites']['versions']['generation-v']['black-white']['animated']['front_default'];
    })
  },
  methods: {
    searchPokemon(){
      // ao procurar o pokemon, antes de executar todo o código abaixo fazer 'iteração de loading'
      this.pokemonName = 'Loading...'
      this.pokemonID = ''

      // ${this.searchValue} pega o valor que foi preenchi do form e o substitui, tanto com nome do pokémon ou id do pokemon
      axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchValue}`)
      
      .then(response => {
        // trazendo o pokemon com o nome buscado ou id buscado
        this.pokemonName = response.data.name;
        this.pokemonID = response.data.id;
        this.pokemonImage = response.data['sprites']['versions']['generation-v']['black-white']['animated']['front_default'];
        this.searchValue = "";
      })
        // caso o id digitado ou nome digitado não exista, executará o bloco de código abaixo
        .catch(error => {
              this.pokemonName = 'Not found :c';
              // com array vazio para retirar a imagem do pokemon já renderizado, deixando a pokedex sem imagem
              this.pokemonImage = [];
            
        })
    },
    // botão de voltar, basicamente pega o id que está na pokedex e subtrai por 1
    btnPrev(){
      axios.get(`https://pokeapi.co/api/v2/pokemon/${(this.pokemonID) - 1 }`)
      .then(response => {
        this.pokemonName = response.data.name;
        this.pokemonID = response.data.id;
        this.pokemonImage = response.data['sprites']['versions']['generation-v']['black-white']['animated']['front_default'];
        this.searchValue = "";
      })
    },
    // botão de avançar, basicamente pega o id que está na pokedex e soma por 1
    btnNext(){
      axios.get(`https://pokeapi.co/api/v2/pokemon/${(this.pokemonID) + 1 }`)
      .then(response => {
        this.pokemonName = response.data.name;
        this.pokemonID = response.data.id;
        this.pokemonImage = response.data['sprites']['versions']['generation-v']['black-white']['animated']['front_default'];
        this.searchValue = "";
      })
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Oxanium:wght@300;400;500;600;700;800&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Oxanium', cursive;
}

body {
  text-align: center;
  background: linear-gradient(to bottom, #6ab7f5, #fff);
  min-height: 100vh;
}

.main {
  display: inline-block;
  margin-top: 2%;
  padding: 15px;
  position: relative;
}

.pokedex {
  width: 100%;
  max-width: 425px;
}

.pokemon__image {
  position: absolute;
  bottom: 55%;
  left: 50%;
  transform: translate(-63%, 20%);
  height: 18%;
}

.pokemon__data {
  position: absolute;
  font-weight: 600;
  color: #aaa;
  top: 54.5%;
  right: 27%;
  font-size: clamp(8px, 5vw, 25px);
}

.pokemon__name {
  color: #3a444d;
  text-transform: capitalize; /*os nomes dos pokemons são trazidos da api todos em minusculo, trás a primeira letra em maiusculo */
}

.form {
  position: absolute;
  background: rgb(231, 231, 231);
  width: 65%;
  top: 65%;
  left: 13.5%;
}

.input__search {
  width: 100%;
  padding: 4%;
  outline: none;
  border: 2px solid #333;
  border-radius: 5px;
  font-weight: 600;
  color: #3a444d;
  font-size: clamp(8px, 5vw, 1rem);
  box-shadow: -3px 4px 0 #888, -5px 7px 0 #333
}

.buttons {
  position: absolute;
  bottom: 10%;
  left: 50%;
  width: 65%;
  transform: translate(-57%, 0);
  display: flex;
  gap: 20px;
}

.button {
  width: 50%;
  padding: 4%;
  border: 2px solid #000;
  border-radius: 5px;
  font-size: clamp(8px, 5vw, 1rem);
  font-weight: 600;
  color: white;
  background-color: #444;
  box-shadow: -2px 3px 0 #222, -4px 6px 0 #000;
}

.button:active {
  box-shadow: inset -4px 4px 0 #222;
  font-size: 0.9rem;
}
</style>

