<template>
    <div class="flex flex-col space-y-4 justify-center items-center">
        <p>Xalapa, Ver</p>
        <p>12:44 PM, Mon, Feb 27, 2023</p>
        <div class="flex justify-center items-center">
            <img src="../assets/Clear.png" alt="">
            <p class="text-5xl font-bold">{{ temperature }}</p>
            <sup class="font-bold text-xl">°C</sup>
        </div>
        <p class="text-5xl font-bold">{{ weather }}</p>
        <div class="m-6 flex space-x-4">
            <Stat name="Humidity" :amount="humidity"/>
            <Stat name="Wind Speed" :amount="windspeed"/>
        </div>
    </div>
</template>

<script setup>
import Stat from "./Stat.vue";

const props = defineProps({
    weather: {
        type: String,
        default: "Clear"
    },
    humidity: {
        type: String,
        default: "0%"
    },
    windspeed: {
        type: String,
        default: "0 km/h"
    },
    temperature: {
        type: Number,
        default: 0
    }
})
</script>