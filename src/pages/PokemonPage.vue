<template>
  <h1 v-if="!pokemon">Please wait...</h1>
  <div v-else>
    <h1>¿Quién es este pokemon?</h1>
    
    <pokemonPicture
      :pokemon-id="pokemon.id"
      :show-pokemon="showPokemon" />
    
    <pokemonOptions 
      :pokemons="pokemonArr"
      @selection="checkAnswer"/>
    
    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">Nuevo Juego</button>
    </template>
  </div>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions'
import PokemonPicture from '@/components/PokemonPicture'

import getPokemonOptions from '@/helpers/getPokemonOptions.js'

export default {
  name: 'PokemonPage',
  components: {
    PokemonOptions,
    PokemonPicture,
    showAnswer: false,
    message: undefined,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false
    }
  },
  methods: {
    async mixPokemonArr() {
      this.pokemonArr = await getPokemonOptions()

      const rndInt = Math.floor( Math.random() * 4 )
      this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer(selectedId) {
      this.showPokemon = true
      this.showAnswer = true

      if (selectedId === this.pokemon.id) {
        this.message = `Fine, ${this.pokemon.name}`
      } else {
        this.message = `OOpss, was ${this.pokemon.name}`
      }
    },
    newGame() {
      this.showPokemon  = false
      this.showAnswer   = false
      this.pokemonArr   = []
      this.pokemon      = null
      this.mixPokemonArr()
    }
  },
  mounted() {
    this.mixPokemonArr()
  }
}
</script>
