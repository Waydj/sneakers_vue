<script setup>
defineProps({ title: String, price: Number, imageUrl: String, onAddToCart: Function })
</script>

<template>
  <div class="flex items-center gap-4 rounded-xl border border-slate-100 p-4">
    <img class="h-16 w-16" :src="imageUrl" alt="sneaker" />

    <div class="flex w-full flex-col">
      <p>{{ title }}</p>

      <div class="mt-2 flex justify-between">
        <b>{{ price }}</b>
        <img
          @click="onAddToCart"
          class="cursor-pointer opacity-30 transition hover:opacity-100"
          src="/close.svg"
          alt="close"
        />
      </div>
    </div>
  </div>
</template>
