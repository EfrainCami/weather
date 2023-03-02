<template>
  <div>
    <apexchart
      width="900"
      height="300"
      type="area"
      :options="options"
      :series="series"
    ></apexchart>
  </div>
</template>

<script setup>
import { ref, onMounted, onUpdated, onBeforeUpdate } from "vue";

const props = defineProps({
  hoursWithTemperatures: {
    type: Array,
    default: [
      [1, 2, 3, 4, 5, 6, 7, 8],
      [1, 2, 3, 4, 5, 6, 7, 8],
    ],
  },
});
let options = null
let series = null

options = {
  chart: {
    id: "vuechart-example",
    toolbar: {
      show: false,
    },
  },
  title: {
    text: "Temperature",
    align: "left",
  },
  dataLabels: {
    enabled: false,
  },
  xaxis: {
    categories: props.hoursWithTemperatures[0],
  },
};
series = [
  {
    name: "series-1",
    data: props.hoursWithTemperatures[1],
  },
]; 

onUpdated(() => {
    options.xaxis.categories = props.hoursWithTemperatures[0]
    series[0].data = props.hoursWithTemperatures[1]
})  
</script> 