<template>
  <div>
    <div v-for="city in savedCities" :key="city.id">
      <CityCard :city="city" @click="goToCityView(city)" />
    </div>

    <p v-if="savedCities.length === 0">
    No locations added. To start tracking a location, search in the field above.</p>
  </div>
</template>

<script setup>
import { ref } from "vue";
import CityCard from "./CityCard.vue";
import axios from "axios";
import { useRouter } from "vue-router";

const savedCities = ref([]);
const getCities = async () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
  }

  const requests = [];
  savedCities.value.forEach((city) => {
    requests.push(
      axios.get(
        `https://api.openweathermap.org/data/2.5/weather?lat=${city.cords.lat}&lon=${city.cords.long}&appid=1d75ae38b3e021d8ce7979a71b564cf9&units=imperial`
      )
    );
  });

  const weatherData = await Promise.all(requests);
  weatherData.forEach((value, index) => {
    savedCities.value[index].weather = value.data;
  });
};

await getCities();
const router = useRouter()
const goToCityView = (city) => {
    router.push({
        name: "cityView",
        params: { state: city.state, city: city.city },
        query: { id: city.id, lat: city.cords.lat, long: city.cords.long }
    })
}
</script>
