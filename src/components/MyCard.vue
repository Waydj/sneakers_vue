<script setup>
defineProps({
  id: Number,
  title: String,
  imgUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function,
  noButtons: Boolean
})
</script>

<template>
  <div
    class="relative cursor-pointer rounded-3xl border border-slate-100 bg-white p-8 transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      v-if="!noButtons"
      @click="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="like 1"
      class="absolute left-8 top-8"
    />
    <img :src="imgUrl" alt="sneaker" />
    <p class="mt-2">{{ title }}</p>

    <div class="mt-5 flex justify-between">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>

      <img
        v-if="!noButtons"
        class="opacity-70 transition hover:opacity-100"
        @click="onClickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="plus"
      />
    </div>
  </div>
</template>
