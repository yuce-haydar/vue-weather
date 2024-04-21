<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['place-data'])


const SearchTerm = reactive({
  query: '',
  timeout: null,
  results:null
})
const handleSearch = async () => {
    clearTimeout(SearchTerm.timeout)
    SearchTerm.timeout = setTimeout(async () => {
        if(SearchTerm.query!==''){
            const res = await fetch(`http://api.weatherapi.com/v1/search.json?key=42702e3048754456a16191624242703&q=${SearchTerm.query}`)  
            const data = await res.json()
            SearchTerm.results = data
            console.log(SearchTerm.results)
        }else{
            SearchTerm.results = null
        }

      
    }, 500)

}
const getWeather =  async (id) => {
    const res = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=42702e3048754456a16191624242703&q=id:${id}&days=3&aqi=no&alerts=no`)
    const data = await res.json()
    emit('place-data',data)
    SearchTerm.results = null
    SearchTerm.query = ''

}
</script>

<template>
  <div>

    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
            v-model="SearchTerm.query"
            @input="handleSearch"
          />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
     <div v-if="SearchTerm.results!==null">
        <div v-for="place in SearchTerm.results" :key="place.id">
        <button @click="getWeather(place.id)" class="p-3  my-2 hover:text-indigo-600 hover:font-bold w-full text-left">
            {{ place.name }} , {{ place.region }} , {{ place.country }}
        </button>
      </div>
     </div>
    </div>
  </div>
</template>
