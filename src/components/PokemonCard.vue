<script>
export default {
  name: 'PokemonCard',
  props: {
    pokemonName: {
      type: String
    },
    imageUrl: {
      type: String
    }
  },
  data() {
    return {
      userInput: '', // adivinación del usuario
      correct: false // variable para saber si ha sido encontrado
    }
  },
  methods: {
    /**
     * Método que se activa cuando queremos enviar información
     */
    sendName() {
      /**
       * Contrasta input usuario con el nombre pokemon
       */
      if (this.userInput.toLowerCase().trim() == this.pokemonName.toLowerCase()) {
        this.correct = true // variable "encontrado" ahora es true
        this.$emit('send-name', this.pokemonName) // envia el nombre del pokemon al padre
      } else {
        alert('Siga participando')
      }
    }
  }
}
</script>
<template>
  <div class="card card-title text-center col-3 align-items-center">
    <img :class="!correct ? 'filter' : ''" :src="imageUrl" :alt="pokemonName" />
    <div v-if="correct">
      <p class="name">{{ pokemonName }}</p>
    </div>
    <form v-else @submit.prevent="sendName">
      <input v-model="userInput" />
      <button>Descubrir</button>
      <p class="incorrect" v-show="correct == false">Incorrecto, Intenta Nuevamente</p>
    </form>
  </div>
</template>
<style scoped>
.filter {
  filter: blur(5px) grayscale(100%);
}
</style>
