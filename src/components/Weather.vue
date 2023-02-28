<template>
    <div class="flex rounded-xl p-4 h-full bg-white">
        <div class="flex flex-col mt-10 items-center w-1/2 space-y-3">
            <Search></Search>
            <Info></Info>
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