<template>
    <div class="flex rounded-xl p-4 h-full bg-white">
        <div class="flex flex-col mt-10 items-center w-1/2 space-y-3">
            <Search></Search>
            <!-- Notar que para los atributos dinámicos no hace falta el .value -->
            <Info :weather="weather" :humidity="humidity" :windspeed="windspeed" :temperature="temperature"></Info>
        </div>
        <div class="w-full">
            <div class="flex items-center justify-center">
                <apexchart width="900" height="300" type="area" :options="options" :series="series"></apexchart>
            </div>
            <div class="grid grid-cols-4 space-x-3">
                <Card day="Today" weather="Clear"></Card>
                <Card day="Feb 28" weather="Clouds"></Card>
                <Card day="Mar 1" weather="Rainy"></Card>
                <Card day="Mar 2" weather="Clear"></Card>
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

const URL = "https://api.openweathermap.org/data/2.5/forecast?lat=19.54&lon=-96.88&cnt=4&units=metric&appid=e1a8068af58a90db438617c593ef93bb";

const weather = ref("--")
const humidity = ref("--")
const windspeed = ref("--")
const temperature = ref(0)

axios.get(URL).then((result) => {
    weather.value = result.data.list[0].weather[0].main;
    humidity.value = (result.data.list[0].main.humidity) + "%"
    windspeed.value = (result.data.list[0].wind.speed) + " km/h"
    temperature.value = result.data.list[0].main.temp
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