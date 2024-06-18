

<script>
import axios from 'axios';
import PokeDiv from './components/PokeDiv.vue';

export default {
  components: {
    PokeDiv
  },
  data() {
    return {
      pokemones: [],
      pokeconteo: 0
    };
  },
  mounted() {
    this.obtenerPokemones();
  },
  methods: {
    async obtenerPokemones() {
      const url = "https://pokeapi.co/api/v2/pokemon";
      try {
        const response = await axios.get(url);
        const data = response.data.results;
        const pokemonDetailsPromises = data.map(async (pokemon) => {
          const pokemonResponse = await axios.get(pokemon.url);
          return {
            nombre: pokemonResponse.data.name,
            url: pokemonResponse.data.sprites.front_default
          };
        });
        Promise.all(pokemonDetailsPromises)
          .then(pokemones => {
            this.pokemones = pokemones;
            this.pokemonesDescubiertos = pokemones.length;
          })
          .catch(error => {
            console.error("Error al obtener los detalles de los pokemones: ", error);
          });
      } catch (error) {
        console.error("Error al obtener los pokemones: ", error);
      }
    },
    incrementarConteo() {
      this.pokeconteo++;
    }
  }
};
</script>
<template>
  <div class="app">
    <header class="pokeHeader">
      <img src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg" alt="">
      <h1>¿Quién es ese pokemon?</h1>
      <h2>Pokemones descubiertos: {{ pokeconteo }}</h2>
    </header>
    <main>
      <PokeDiv v-for="(pokemon, index) in pokemones" :key="index" :nombre="pokemon.nombre" :url="pokemon.url" @descubierto="incrementarConteo"/>
    </main>
  </div>
</template>
<style scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 20px;
}

.pokeHeader {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 20px;
}

main {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
</style>
