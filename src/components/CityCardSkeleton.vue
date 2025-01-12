<template>
  <div class="flex py-6 px-4 bg-weather-secondary rounded-md shadow-md">
    <div class="flex flex-col flex-1 gap-2">
      <AnimatedPlaceholder class="max-w-[50%]" />
      <AnimatedPlaceholder class="max-w-[40%]" />
    </div>

    <div class="flex flex-col items-end gap-2">
      <AnimatedPlaceholder class="max-w-[50px] w-full" />
      <AnimatedPlaceholder class="max-w-[40px] w-full" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from "@/components/AnimatedPlaceholder.vue";
</script>

