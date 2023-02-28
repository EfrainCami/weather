<template>
    <div class="flex flex-col space-y-3 items-center">
        <p class="text-gray-500">{{ name }}</p>
        <p>{{ amount }}</p>
    </div>
</template>

<script setup>

const props = defineProps({
    name: {
        type: String,
        default: "stat"
    },
    amount: {
        type: String,
        default: "000"
    }
})
</script>