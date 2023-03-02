<template>
    <div class="flex p-4 rounded-xl bg-white">
        <div class="flex flex-col mt-10 items-center w-1/2 space-y-3">
            <Search></Search>
            <!-- Notar que para los atributos dinámicos no hace falta el .value -->
            <Info :city="city" :date="date" :weather="weather" :humidity="humidity" :windspeed="windspeed" :temperature="temperature"></Info>
        </div>
        <div class="w-full">
            <div class="flex items-center justify-center">
                <Chart></Chart>
                
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
import Chart from "./Chart.vue"

import { ref, onMounted } from 'vue';

const URL = "https://api.openweathermap.org/data/2.5/forecast?lat=19.52&lon=-96.93&cnt=4&units=metric&appid=e1a8068af58a90db438617c593ef93bb";
const newURL = "https://api.openweathermap.org/data/2.5/forecast?q=Monterrey&units=metric&cnt=32&appid=e1a8068af58a90db438617c593ef93bb";

const weather = ref("--")
const humidity = ref("--")
const windspeed = ref("--")
const date = ref("--")
const temperature = ref(0)
const city = ref("--")

const indexClicked = ref(0)

let forecastArray = []

const showInfo = (id) =>{
    weather.value = forecastArray[id].weather[0].main
    humidity.value = (forecastArray[id].main.humidity) + "%"
    windspeed.value = (forecastArray[id].wind.speed) + " km/h"
    temperature.value = forecastArray[id].main.temp
    date.value = forecastArray[id].dt_txt

    indexClicked.value = id
}

let dayOneTemps = []
let dayTwoTemps = []
let dayThreeTemps = []
let dayFourTemps = []

axios.get(newURL).then((result) => {
    for (let i = 0; i <= 24; i+=8) {
        forecastArray.push(result.data.list[i]) 
    }

    for (let i = 0; i < 32; i++) {
        if (i <= 7) {
            dayOneTemps.push(result.data.list[i].main.temp)    
        } else if (i <= 15){
            dayTwoTemps.push(result.data.list[i].main.temp)    
        } else if (i <= 23){
            dayThreeTemps.push(result.data.list[i].main.temp)    
        } else {
            dayFourTemps.push(result.data.list[i].main.temp)    
        }
    }
    city.value = result.data.city.name
    showInfo(0)
    console.log(result.data)
    console.log(dayOneTemps)
    console.log(dayTwoTemps)
    console.log(dayThreeTemps)
    console.log(dayFourTemps)
    
})


</script>