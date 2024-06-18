
<script>
export default {
  name: 'PokeDiv',
  props: ['nombre', 'url'],
  data() {
    return {
      pokeadivinar: '',
      descubierto: false,
    };
  },
  methods: {
    descubrirPokemon() {
      if (this.pokeadivinar.toLowerCase() === this.nombre.toLowerCase()) {
        this.descubierto = true;
        this.pokeadivinar= '';
        this.$emit('descubierto');
      } else {
        alert('No es ese pokemón');
        this.descubierto = false;
        this.pokeadivinar= '';
      }
      
    }
  }
};
</script>
<template>
  <div class="pokeDiv">
    <img :src="url" :alt="'Imagen de ' + nombre" :class="{ blurred: !descubierto}">
    <h3 v-show="descubierto">{{ nombre }}</h3>
    <div v-show="!descubierto" class="pokeForm">
      <input
      type="text"
      placeholder="Ingresa el pokemón"
      v-model="pokeadivinar"
      @keydown.enter="descubrirPokemon"
    >
    <button @click="descubrirPokemon">Descubrir</button>
    </div>
   
  </div>
</template>
<style scoped>
.pokeDiv {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 10px;
  text-align: center;
  width: 50vh;
  
}

img {
  width: 100%;
  max-width: 100%;
  height: auto;
}
.pokeForm {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

button {
  width: 100%;
}

.blurred {
  filter: blur(5px) grayscale(100%);
}
h3{
  margin: 0;
}
</style>
