<script>
import axios from 'axios'
import PokemonCard from '@/components/PokemonCard.vue'

export default {
  name: 'PokemonList',
  components: { PokemonCard },
  data() {
    return {
      /** Arreglo pokemon vacío */
      pokemonList: []
    }
  },
  /** Hacemos llamado al API */
  async mounted() {
    try {
      for (let i = 1; i <= 20; i++) {
        const url = 'https://pokeapi.co/api/v2/pokemon/' + i
        const { data } = await axios.get(url)
        this.pokemonList.push(data)
      }
    } catch (error) {
      console.error('No pudimos atrapar un pokemon 🥺')
    }
  },
  computed: {
    counter() {
      return this.pokemonList.filter((pokemon) => pokemon.esCorrecto).length
    }
  },
  methods: {
    /**
     * Se activa cuando el hijo envia el nombre del
     * pokemon encontrado
     **/
    discovered(pokemonName) {
      const foundPokemon = this.pokemonList.find(
        (pokemon) => pokemon.name.toLowerCase() == pokemonName
      )
      /** Al pokemon encontrado le agregamos la prop esCorrecto */
      foundPokemon.esCorrecto = true
    }
  }
}
</script>
<template>
  <div class="container">
    <div class="mt-3 d-flex flex-column align-items-center">
      <img src="/public/International_Pokémon_logo.svg.png" alt="logo_Pokemon" />
      <h1>¿Quien es este Pokemon?</h1>
      <p>Pokemones encontrados: {{ counter }}</p>
    </div>
    <div class="row mt-3">
      <PokemonCard
        v-for="(pokemon, idx) in pokemonList"
        :key="idx"
        :pokemonName="pokemon.name"
        :imageUrl="pokemon.sprites.front_default"
        @send-name="discovered"
      />
    </div>
  </div>
</template>
<style scoped></style>
