<script setup>
import MyCard from './MyCard.vue'

defineProps({
  items: Array
})
</script>

<template>
  <div class="mt-10 grid grid-cols-4 gap-5">
    <MyCard
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :img-url="item.imageUrl"
      :price="item.price"
      :isAdded="false"
      :isFavorite="false"
    />
  </div>
</template>
