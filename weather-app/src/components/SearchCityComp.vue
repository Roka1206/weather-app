<template>
  <div>
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for City"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query"
          @input="handleSearch"
        />
      </div>
    </form>
    <div v-if="searchTerm.result !== null">
      <div class="bg-white my-2 rounded-lg shadow-lg">
        <div v-for="city in searchTerm.result" :key="city.id">
          <button
            @click="getWeather(city.id)"
            class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
          >
            {{ city.name }}, {{ city.region }}, {{ city.country }},
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['city-data'])

const searchTerm = reactive({
  query: '',
  timeout: null,
  result: null
})

const handleSearch = () => {
  clearTimeout(searchTerm.timeout)
  searchTerm.timeout = setTimeout(async () => {
    if (searchTerm.query !== '') {
      const res = await fetch(
        `http://api.weatherapi.com/v1/search.json?key=eb06477bd88b42da853185042241002&q=${searchTerm.query}`
      )
      const data = await res.json()
      searchTerm.result = data
    } else {
      searchTerm.result = null
    }
  }, 500)
}

const getWeather = async (id) => {
  const res =
    await fetch(`http://api.weatherapi.com/v1/forecast.json?key=eb06477bd88b42da853185042241002&q=id:${id}&days=3&aqi=no&alerts=no
`)
  const data = await res.json()
  emit('city-data', data)
}

searchTerm.query = ''
searchTerm.result = null
</script>

<style scoped></style>
