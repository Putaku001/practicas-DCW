<script setup>
import{ref, computed} from 'vue';
import axios from 'axios'

const nombrePokemon = ref('')
const pokemon = ref(null)
const error = ref('')

const tipos = computed(() =>{
    return pokemon.value?.types?.map(t => t.type.name) || []
})


const buscarPokemon = async () => {
    error.value =  ""
    pokemon.value = null


if(!nombrePokemon.value.trim()){
    error.value =  "what is pokemon!"
}
try {
    const res = await axios.get(`https://pokeapi.co/api/v2/pokemon/${nombrePokemon.value.toUpperCase()}`)
    //if(!res.ok) throw new Error("el pokemon no a sido encontrado en la pokedex")


    pokemon.value = res.data
} catch (e) {
    error.value = e.response?.status === 404
    ? "Pokemon no encontrado en la pokedex"
    : "error al buscar"
}
}


</script>

<template>
    <div class="p-4 max-w-md mx-auto">
        <input class="border p-2 rounded w-full" v-model="nombrePokemon" type="text" placeholder="ingresa el nombre de un pokemon">
        <button @click="buscarPokemon" class="mt-2 bg-red-600 text-white px-4 py-2 rounded hover:bg-red-500">Buscar</button>

        <div v-if="pokemon" class="mt-6 bg-white shadow-md rounded p-4 text-center">
            <img :src="pokemon.sprites.front_default" alt="imagen" class="mx-auto mb-4 h-1/2">
            <h2 class="text-xl font-bold capitalize">{{ pokemon.name }}</h2> 
            <p>Altura: {{ pokemon.height }} mts</p>
            <p>Peso: {{ pokemon.weight }} lbs</p>
            <p>
                tipo: 
                <span v-for="tipo in tipos" :Key="tipo" class="inline-block gb-gray rounded">
                  {{ tipo }}
                </span>
            </p>
        </div>
        <p v-if="error" class="text-red ">{{ error }}</p>
    </div>
</template>


