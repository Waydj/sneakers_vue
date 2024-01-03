<script setup>
defineProps({
  title: String,
  description: String,
  img: String
})
</script>

<template>
  <div class="flex h-full flex-col items-center justify-center">
    <img height="100" width="100" :src="img" alt="image" />
    <h2 class="mt-4 text-center text-2xl font-medium">{{ title }}</h2>
    <p class="mt-2 text-center text-lg text-slate-500">{{ description }}</p>
  </div>
</template>
