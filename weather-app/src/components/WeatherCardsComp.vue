<template>
  <div
    :class="city.current.is_day === 1 ? 'bg-day' : 'bg-night'"
    class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden"
  >
    <div class="mb-2 flex justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3x1">{{ city.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3x1">
          {{ new Date(city.location.localtime).getHours() }}:
          {{ new Date(city.location.localtime).getMinutes() }}
        </h1>
      </div>
    </div>

    <div class="text-center flex-">
      <img :src="city.current.condition.icon" alt="icon" width="200" class="mx-auto -mb10" />
      <h1 class="text-9x1 mb-2">{{ Math.round(city.current.temp_c) }}&deg;</h1>
      <p class="text-2x1">{{ city.current.condition.text }}</p>
    </div>

    <BorderLineComp />

    <div v-for="(day, idx) in city.forecast.forecastday" :key="idx">
      <WeatherForDayComp :day="day" />
    </div>

    <Transition name="fade">
      <div v-show="showDetail">
        <WeatherInfoComp
          :city="city"
          @close-info="showDetail = false"
          @remove-city="removeCity(city.location.name)"
        />
      </div>
    </Transition>

    <div class="flex justify-end items-center gap-1 mt-10">
      <button @click="showDetail = true">
        More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import BorderLineComp from './BorderLineComp.vue'
import WeatherForDayComp from './WeatherForDayComp.vue'
import WeatherInfoComp from './WeatherInfoComp.vue'

defineProps({
  city: Object
})

const emit = defineEmits(['delete-city'])

const showDetail = ref(false)

const removeCity = (cityName) => {
  emit('delete-city', cityName)
  showDetail.value = false
}
</script>

<style scoped>
.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}
.bg-night {
  background-color: #07223d;
  background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
