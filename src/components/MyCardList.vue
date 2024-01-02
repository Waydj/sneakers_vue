<script setup>
import { inject } from 'vue'
import MyCard from './MyCard.vue'

defineProps({
  items: Array
})

const addToFavorites = inject('addToFavorites')
</script>

<template>
  <div class="mt-10 grid grid-cols-4 gap-5">
    <MyCard
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :img-url="item.imageUrl"
      :price="item.price"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :onClickFavorite="() => addToFavorites(item)"
    />
  </div>
</template>
