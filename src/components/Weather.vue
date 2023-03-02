<template>
  <div class="flex p-4 rounded-xl bg-white">
    <div class="flex flex-col mt-10 items-center w-1/2 space-y-3">
      <Search></Search>
      <!-- Notar que para los atributos dinámicos no hace falta el .value -->
      <Info
        :city="city"
        :date="date"
        :weather="weather"
        :humidity="humidity"
        :windspeed="windspeed"
        :temperature="temperature"
      ></Info>
    </div>
    <div class="w-full">
      <div class="flex items-center justify-center">
        <Chart :hoursWithTemperatures="hoursWithTemperatures"></Chart>
      </div>
      <div class="grid grid-cols-4 space-x-3">
        <Card
          @:click="showInfo(index)"
          v-for="(forecast, index) in forecastArray"
          :key="index"
          day="Today"
          :id="index"
          :weather="forecast.weather[0].main"
          :humidity="forecast.main.humidity"
          :indexClicked="indexClicked"
          :temperatures="daysTemperatures[index]"
          :hours="daysHours[index]"
          @hoursWithTemperatures="
            (hoursWithTemps) => (hoursWithTemperatures = hoursWithTemps)
          "
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import Card from "./Card.vue";
import Info from "./Info.vue";
import Search from "./Search.vue";
import axios from "axios";
import Chart from "./Chart.vue";

import { ref, onMounted, onUpdated } from "vue";

const newURL =
  "https://api.openweathermap.org/data/2.5/forecast?q=Xalapa&units=metric&cnt=32&appid=e1a8068af58a90db438617c593ef93bb";

const weather = ref("--");
const humidity = ref("--");
const windspeed = ref("--");
const date = ref("--");
const temperature = ref(0);
const city = ref("--");

const hoursWithTemperatures = ref([]);

const indexClicked = ref(0);

let forecastArray = [];

let dayOneTemps = [];
let dayTwoTemps = [];
let dayThreeTemps = [];
let dayFourTemps = [];

let daysTemperatures = [];

let dayOneHours = [];
let dayTwoHours = [];
let dayThreeHours = [];
let dayFourHours = [];

let daysHours = [];

onMounted(() => {
  axios.get(newURL).then((result) => {
    populateTemperatures(result);
    city.value = result.data.city.name;
    showInfo(0);
  });
});

const populateTemperatures = (result) => {
  for (let i = 0; i <= 24; i += 8) {
    forecastArray.push(result.data.list[i]);
  }
  for (let i = 0; i < 32; i++) {
    if (i <= 7) {
      dayOneTemps.push(result.data.list[i].main.temp);
      dayOneHours.push(result.data.list[i].dt_txt);
    } else if (i <= 15) {
      dayTwoTemps.push(result.data.list[i].main.temp);
      dayTwoHours.push(result.data.list[i].dt_txt);
    } else if (i <= 23) {
      dayThreeTemps.push(result.data.list[i].main.temp);
      dayThreeHours.push(result.data.list[i].dt_txt);
    } else {
      dayFourTemps.push(result.data.list[i].main.temp);
      dayFourHours.push(result.data.list[i].dt_txt);
    }
  }
  daysTemperatures = [dayOneTemps, dayTwoTemps, dayThreeTemps, dayFourTemps];
  daysHours = [dayOneHours, dayTwoHours, dayThreeHours, dayFourHours];
};

const showInfo = (id) => {
  weather.value = forecastArray[id].weather[0].main;
  humidity.value = forecastArray[id].main.humidity + "%";
  windspeed.value = forecastArray[id].wind.speed + " km/h";
  temperature.value = forecastArray[id].main.temp;
  date.value = forecastArray[id].dt_txt;

  indexClicked.value = id;
};

console.log(hoursWithTemperatures._value);
</script>
