<template>
  <div class="flex flex-col flex-1 items-center">
    <!-- Banner -->
      <div 
        v-if="route.query.preview" 
        class="text-white p-4 bg-weather-secondary w-full text-center"
      >
        <p>You are currently previewing the city, click the "+" icon to start tracking this city.</p>
      </div>
    <!-- Weather OverView -->
     <div class="flex flex-col items-center text-white py-12">
      <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
      <p class="text-sm mb-12">
        {{ 
          new Date(weatherData.currentTime).toLocaleString('en-US', {
            weekday: 'short',
            month: 'long',
            day: '2-digit',
          }) 
        }}
        {{ 
          new Date(weatherData.currentTime).toLocaleString('en-US', {
            timeStyle: 'short',
          }) 
        }}
      </p>
      <p class="text-8xl mb-8">
        {{ Math.round(weatherData.main.temp - 273.15) }}°C
      </p>
        <p>Feels like {{ Math.round(weatherData.main.feels_like - 273.15) }}°C</p>
        <p class="capitalize">{{ weatherData.weather[0].description }}</p>
        <img
          class="w-[150px] h-auto"
          :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
          :alt="weatherData.weather[0].description" 
        />
     </div>

     <hr class="border-white border-opacity-10 border w-full" />

     <!-- Hourly Weather -->
     <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-2">Hourly Weather</h2>
        <div class="flex gap-10 overflow-x-scroll">
          <div 
            v-for="hourData in weatherData.hourly" 
            :key="hourData.dt"
            class="flex flex-col gap-4 items-center"
          >
            <p class="whitespace-nowrap text-md">
              {{ 
                new Date(
                  hourData.currentTime
                ).toLocaleDateString('en-US', {
                  hour: 'numeric',
                })
              }}
            </p>
          </div>
        </div>
      </div>
     </div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const APIKey = import.meta.env.VITE_API_KEY; // Securely import API key from .env


const getWeatherData = async () => {
  const city = route.query.city || 'default_city'; // Add a default value to avoid undefined errors
  const state = route.query.state || 'default_state';
  try {
    const response = await axios.get(
      `http://api.openweathermap.org/data/2.5/weather?q=${city},${state}&appid=${APIKey}`
    );

    const weatherData = response.data;

    // Fetch hourly weather data
    

    // Calculate current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.dt * 1000 + localOffset;
    weatherData.currentTime = utc + 1000 * weatherData.timezone;

    return weatherData;
  } catch (error) {
    console.error('Error fetching weather data:', error);
  }
};

const weatherData = await getWeatherData();
console.log(weatherData);
</script>
