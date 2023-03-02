<template>
    <div class="flex flex-col items-center p-4 rounded-xl cursor-pointer"
        :class="{ 'bg-blue-500': clicked, 'bg-gray-100': !clicked }">
        <p class="font-bold" :class="{ 'text-white': clicked, 'text-black': !clicked }">{{ day }}</p>
        <img class="h-10 m-6" :src="imageName" alt="Weather">
        <p :class="{ 'text-white': clicked, 'text-black': !clicked }">Humidity</p>
        <p :class="{ 'text-white': clicked, 'text-black': !clicked }">{{ humidity }}%</p>
    </div>
</template>

<script setup>
import { ref, onMounted, onUpdated } from 'vue';

const props = defineProps({
    day: {
        type: String,
        default: "Today"
    },
    weather: {
        type: String,
        default: "Clear"
    },
    selected: {
        type: Boolean,
        default: false
    },
    humidity: {
        type: Number,
        default: 0
    },
    id: {
        type: Number,
        default: 0
    },
    indexClicked: {
        type: Number,
        default: 99
    }
})

const imageName = ref("src/assets/Clear.png")

onMounted(() => {
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


const clicked = ref(false)
onUpdated(() => {
    clicked.value = props.id === props.indexClicked
})

</script>