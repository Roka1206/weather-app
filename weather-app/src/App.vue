<template>
  <main>
    <!--     <DateComp /> -->
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString('en-us', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>
    <div>
      <SearchCityComp @city-data="addCity" />
    </div>
    <div class="grid grid-cols-2 gap-4">
      <div v-for="city in citys" :key="city.id">
        <WeatherCardsComp :city="city" @delete-city="deleteCity" />
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import SearchCityComp from './components/SearchCityComp.vue'
import WeatherCardsComp from './components/WeatherCardsComp.vue'

const citys = ref([])

const addCity = (data) => {
  console.log(data)
  citys.value.push(data)
}

const deleteCity = (name) => {
  if (confirm('Are you sure')) {
    citys.value = citys.value.filter((c) => c.location.name !== name)
  }
}
</script>

<style scoped></style>
