<script setup>
import SearchInput from './components/SearchInput.vue'
import WeatherCard from './components/WeatherCard.vue'
import { ref } from 'vue'
const places = ref([])
const addPlace = (data) => {
 places.value.push(data)
}
</script>
<template>
  <main>     
    <!-- Date -->
    <div class="text-center mb-6">
 
      {{ new Date().toLocaleString('tr-TR', {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      })}}
    </div>
    <!-- search -->
    <div>
      <SearchInput @place-data="addPlace" />
    </div>
    <!-- weather card -->
    <div class="grid grid-cols-2 gap-4">
      <div v-for="(place,idx) in places" :key="idx">
      <WeatherCard  :place="place"/>
    </div>
    </div>
  </main>
</template>