<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input 
        type="text"
        v-model="searchQuery" 
        @input="getSearchResults"
        placeholder="Search for a city" 
        class="w-full p-2 bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71] text-white" 
      />
      <ul 
        class="absolute bg-weather-secondary text-white w-full shadow-md p-2 top-[66px]"
        v-if="searchResults"
      >
        <p v-if="searchError">Something went wrong, please try again.</p>
        <p v-if="!serverError && searchResults.length === 0">No result match your query</p>
        <template v-else>
          <li
            v-for="searchResult in searchResults"
            :key="searchResult.local_name"
            class="py-2 cursor-pointer"
          >
          {{ searchResult.local_name }} ({{ searchResult.state }}, {{ searchResult.country }})
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const APIKey = '4c3399e8149d460dc52d068469f89c91';

const searchQuery = ref('');
const queryTimeout = ref(null);
const searchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      try {
        const result = await axios.get(`http://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${APIKey}`);
        searchResults.value = result.data.map((place) => ({
          local_name: place.name,
          country: place.country,
          state: place.state,
        }));
        console.log(searchResults.value); // Debugging response
      } catch (error) {
        console.error("Error fetching weather data:", error);
        searchError.value = true;
      }
    } else {
      searchResults.value = null; // Clear results if the search query is empty
    }
  }, 500);
};
</script>
