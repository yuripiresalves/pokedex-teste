<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form @submit.prevent>
          <input
            class="form-control form-control-lg"
            type="text"
            v-model="pokemon"
            required
          />
          <button
            type="submit"
            class="btn btn-primary mt-3"
            @click="getPokemon"
          >
            Ver Pokémon
          </button>
        </form>
        <div class="row">
          <div class="col">
            <div v-if="loading" class="spinner-border mt-4" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
            <div v-if="pokemonData">
              <div class="card mt-5" style="width: 18rem;">
                <img
                  :src="pokemonData.image"
                  class="card-img-top"
                  alt="Pokémon"
                />
                <div class="card-body">
                  <h5 class="card-title">{{ pokemonData.name }}</h5>
                  <p class="card-text">
                    {{ pokemonData.name }} é do tipo {{ pokemonData.type }}
                  </p>
                  <button @click="pokemonData = ''" class="btn btn-danger">
                    Fechar
                  </button>
                </div>
              </div>
            </div>
          </div>
          <div v-if="error" class="col-12 mt-3">
            Deu erro mané
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pokemon: "",
      pokemonData: "",
      error: false,
      loading: false
    };
  },
  methods: {
    async getPokemon() {
      if (this.pokemon === "") return;
      this.loading = true;
      try {
        const response = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${this.pokemon}`
        );
        const json = await response.json();
        const { id, name, types, sprites } = json;
        this.pokemonData = {
          id,
          name,
          type: types[0].type.name,
          image: sprites.other["official-artwork"].front_default
        };
        this.error = false;
        this.pokemon = "";
        this.loading = false;
      } catch (error) {
        this.pokemon = "";
        this.pokemonData = "";
        this.error = true;
        this.loading = false;
      }
    }
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
