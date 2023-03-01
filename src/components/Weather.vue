<template>
    <div class="flex p-4 rounded-xl bg-white">
        <div class="flex flex-col mt-10 items-center w-1/2 space-y-3">
            <Search></Search>
            <!-- Notar que para los atributos dinámicos no hace falta el .value -->
            <Info :city="city" :date="date" :weather="weather" :humidity="humidity" :windspeed="windspeed" :temperature="temperature"></Info>
        </div>
        <div class="w-full">
            <div class="flex items-center justify-center">
                <apexchart width="900" height="300" type="area" :options="options" :series="series"></apexchart>
            </div>
            <div class="grid grid-cols-4 space-x-3">
                <Card @:click="showInfo(index)" v-for = "(forecast, index) in forecastArray" :key="index" day="Today" :id="index" :weather="forecast.weather[0].main" :humidity="forecast.main.humidity" :indexClicked="indexClicked"></Card>
            </div>
        </div>
    </div>
</template>
  
<script setup>
import Card from "./Card.vue";
import Info from "./Info.vue"
import Search from "./Search.vue";
import axios from "axios";

import { ref, onMounted } from 'vue';

const URL = "https://api.openweathermap.org/data/2.5/forecast?lat=19.52&lon=-96.93&cnt=4&units=metric&appid=0c5a1a43510d752c2001201ac1dae836";

const weather = ref("--")
const humidity = ref("--")
const windspeed = ref("--")
const date = ref("--")
const temperature = ref(0)
const city = ref("--")

const indexClicked = ref(0)

let forecastArray = null

const showInfo = (id) =>{
    weather.value = forecastArray[id].weather[0].main
    humidity.value = (forecastArray[id].main.humidity) + "%"
    windspeed.value = (forecastArray[id].wind.speed) + " km/h"
    temperature.value = forecastArray[id].main.temp
    date.value = forecastArray[id].dt_txt

    indexClicked.value = id
}

axios.get(URL).then((result) => {
    forecastArray = result.data.list
    city.value = result.data.city.name
    showInfo(0)
    console.log(result.data)
})

const options = {
    chart: {
        id: 'vuechart-example',
        toolbar: {
            show: false,
        }
    },
    title: {
        text: 'Temperature',
        align: 'left'
    },
    dataLabels: {
        enabled: false
    },
    xaxis: {
        categories: ["8:00", "9:00", "10:00", "11:00", "12:00", "13:00", "14:00"],
    },
}

const series = [{
    name: 'series-1',
    data: [20, 20, 30, 32, 30, 30, 25],
}]
</script>