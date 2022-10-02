<script setup>
import { ref } from 'vue';

const apiKey = '44cf9d1bfa9c6f194e8c16effe4c6611';
const apiBase = 'https://api.openweathermap.org/data/2.5/weather?q=';
const query = ref('');
const weather = ref({});

const setResults = (res) => {
   weather.value = res;
};

const fetchWeather = (e) => {
   if (e.key == 'Enter') {
      fetch(`${apiBase}${query.value}&units=metric&appid=${apiKey}`)
         .then((res) => {
            return res.json();
         })
         .then(setResults);
   }
};

const dateBuiled = () => {
   let d = new Date();
   let months = [
      'January','February','March','April','May','June','July','August','September','October','November','December',
   ];
   let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

   let day = days[d.getDay()];
   let date = d.getDate();
   let month = months[d.getMonth()];
   let year = d.getFullYear();
   return `${day} ${date} ${month} ${year}`;
};
</script>

<template>
   <main class="app" :class="typeof weather.main !== 'undefined' && weather.main.temp > 18 ? 'warm' : ''">
      <div class="search-box">
         <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
         <div class="location-box">
            <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="date">{{ dateBuiled() }}</div>
         </div>
         <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
         </div>
      </div>
   </main>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito&display=swap');

* {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
}

body {
   font-family: 'Nunito', sans-serif;
}

.app {
   background-image: url('./assets/cold-bg.avif');
   background-size: cover;
   transition: 0.4s;
}

.app.warm {
   background-image: url('./assets/warm-bg.avif');
}

main {
   min-height: 100vh;
   padding: 25px;

   background-image: li;
}

.search-box {
   width: 100%;
   margin-bottom: 30px;
}

.search-bar {
   display: block;
   width: 100%;
   padding: 15px;

   color: #313131;
   font-size: 20px;

   appearance: none;
   border: none;
   outline: none;
   background: none;

   box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
   background-color: rgba(255, 255, 255, 0.5);
   border-radius: 0px 16px 0px 16px;
   transition: 0.4s;
}

.search-bar:focus {
   background-color: rgba(255, 255, 255, 0.75);
   box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
   border-radius: 16px 0px 16px 0px;
}

/* WEATHER */

.location {
   color: #fff;
   font-size: 32px;
   font-weight: 500;
   text-align: center;
   text-shadow: 1px 2px rgba(0, 0, 0, 0.2);
}

.date {
   color: #fff;
   font-size: 20px;
   font-weight: 200;
   font-style: italic;
   text-align: center;
}

.weather-box {
   text-align: center;
}

.temp {
   display: inline-block;
   padding: 10px 25px;
   color: #fff;
   font-size: 102px;
   font-weight: 900;

   text-shadow: 2px 3px rgba(0, 0, 0, 0.2);
   background-color: rgba(255, 255, 255, 0.25);
   border-radius: 16px;
   margin: 30px 0;
   box-shadow: 2px 2px rgba(0, 0, 0, 0.2);
}

.weather {
   color: #fff;
   font-size: 48px;
   font-weight: 200;
   text-shadow: 2px 2px rgba(0, 0, 0, 0.2);
}
</style>
