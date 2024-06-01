<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-primary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />

      <ul
        v-if="mapboxSearchResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      >
        <p v-if="searchError">Sorry, something went wrong, please try again.</p>
        <p v-if="!searchError && mapboxSearchResults.length === 0">
          No results match your query, try a different term.
        </p>
        <Template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
          >
            {{ searchResult.place_name }}
          </li>
        </Template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const mapboxApiKey =
  "pk.eyJ1IjoiYWxpcmF6YTEyMzQiLCJhIjoiY2x3dzg2c3NvMHIwcDJ6cjFsdWxlbTl3cCJ9.sdIuiqJawFPuy88eaC8CDg";
const searchQuery = ref("");
const queryTimeOut = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeOut.value);
  queryTimeOut.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxApiKey}`
        );

        mapboxSearchResults.value = result.data.features;
        console.log(result.data.features);
        return;
      } catch (error) {
        searchError.value = true;
      }
    }

    mapboxSearchResults.value = null;
  }, 300);
};
</script>
