<template>
    <div>
        <apexchart width="900" height="300" type="area" :options="options" :series="series"></apexchart>
    </div>
</template>

<script setup>
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

