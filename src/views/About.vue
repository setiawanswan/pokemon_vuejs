<template>
  <div class="about">
    <!-- <h1>This is an about page</h1>
    <h3>{{$route.params.slug}}</h3> -->
    <div 
      v-if="pokemon"
      class="w-6/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
    >
        <h3 class="text-2xl text-green-900 uppercase"> {{ pokemon.name }} </h3>
        <div class="flex justify-center">
          <img class="w-48" :src="pokemon.sprites.front_shiny" alt="">
          <img class="w-48" :src="pokemon.sprites.back_shiny" alt="">
        </div>
        <h3 class="text-yellow-400">Type</h3>
        <div v-for="(type, idx) in pokemon.types" :key="idx">
          <h5 class="text-blue-500">{{type.type.name}}</h5>
        </div>
    </div>
  </div>
</template>

<script>
import {useRoute} from 'vue-router'
import {reactive, toRefs} from 'vue'

export default {

  setup() {
    const route = useRoute()

    const state = reactive({
      pokemon: null
    })

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
    .then((res) => res.json())
    .then((data) => {
      console.log(data)
      state.pokemon = data;
    })

    return {...toRefs(state)}
  },
}
</script>
