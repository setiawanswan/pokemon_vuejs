<template>
  <div class="w-full flex justify-center">
    <input 
      type="text" placeholder="Enter Pokemon Here !" 
      class="mt-10 p-2 border-blue-500 border-2"
      v-model="text"
    >
  </div>
  <div class="mt-10 p-4 grid grid-flow-col grid-cols-3 grid-rows-5 gap-4 justify-center">
    <div 
      class="ml-4 text-2x text-blue-500"
      v-for="(pokemon, idx) in filteredPokemon"
      :key="idx"
    >
    <router-link :to="`/about/${urlIdLookUp[pokemon.name]}`">
        {{ pokemon.name }} 
    </router-link>
    </div>
  </div>
  <!-- <div class="home">
    <h3>Hello World</h3>
    {{ pokemons }}
  </div> -->
</template>

<script>
import {computed, reactive, toRefs} from 'vue';

export default {
  name: 'Home',
  setup(){

    const state = reactive({
      pokemons: [],
      urlIdLookUp: {},
      text: '',
      filteredPokemon:computed(() => updatePokemon())
    })

    function updatePokemon(){
      if(!state.text){
        return state.pokemons
      }

      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      )
    }

    fetch("https://pokeapi.co/api/v2/pokemon")
    .then((res) => res.json())
    .then((data) => {
      console.log(data)
      state.pokemons = data.results
      state.urlIdLookUp = data.results.reduce((acc, cur, idx)=>
      acc = {...acc, [cur.name]:idx+1}
      ,{})
    })

    return {...toRefs(state)}

  }
}
</script>
