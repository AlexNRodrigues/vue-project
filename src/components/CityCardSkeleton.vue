<template>
    <div class="flex py-6 px-3 bg-weather-secondary rounded-md shadow-md">
        <div class="flex flex-col flex-1 gap-2">
            <AnimatedPlaceholder class="max-w-[50%]"></AnimatedPlaceholder>
            <AnimatedPlaceholder class="max-w-[50%]"></AnimatedPlaceholder>
        </div>

        <div class="flex flex-col items-center flex-1 gap-2">
            <AnimatedPlaceholder class="max-w-[50px] w-full"></AnimatedPlaceholder>
            <AnimatedPlaceholder class="max-w-[75px] w-full"></AnimatedPlaceholder>
        </div>
    </div>
</template>

<script setup>
import AnimatedPlaceholder from './AnimatedPlaceholder.vue';


</script>

<style lang="scss" scoped>

</style>