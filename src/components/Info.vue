<template>
    <div class="flex flex-col space-y-4 justify-center items-center">
        <p>{{ city }}</p>
        <p>{{ date }}</p>
        <div class="flex justify-center items-center">
            <img :src="imageName" alt="">
            <p class="text-5xl font-bold">{{ temperature }}</p>
            <sup class="font-bold text-xl">°C</sup>
        </div>
        <p class="text-5xl font-bold">{{ weather }}</p>
        <div class="m-6 flex space-x-4">
            <Stat name="Humidity" :amount="humidity" />
            <Stat name="Wind Speed" :amount="windspeed" />
        </div>
    </div>
</template>

<script setup>
import { ref, onUpdated } from 'vue';
import Stat from "./Stat.vue";

const props = defineProps({
    date: {
        type: String,
        default: "0000-00-00"
    },
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
    },
    city: {
        type: String,
        default: "---"
    }
})

const imageName = ref("src/assets/Clear.png")

onUpdated(() => {
    switch (props.weather) {
        case "Clear":
            imageName.value = "src/assets/" + "Clear" + ".png"
            break;
        case "Clouds":
            imageName.value = "src/assets/" + "Clouds" + ".png"
            break;
        case "Rainy":
            imageName.value = "src/assets/" + "Rain" + ".png"
            break;
        default:
            break;
    }    
})


</script>