<template>
  <div class="w-full flex justify-center">
    <input
      v-model="text"
      type="text"
      placeholder="Enter Pokemon here"
      class="mt-10 p-2 border-blue-500 border-2"
    />
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div
      class="ml-4 text-2x text-blue-500"
      v-for="(pokemon, idx) in fiteredPokemon"
      :key="idx"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </router-link>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from "vue";
export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: "",
      fiteredPokemon: computed(() => updatePokemon()),
    });
    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }
    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => res.json())
      .then((data) => {
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      });
    return { ...toRefs(state) };
  },
};
</script>
