<script setup>
import MyCard from './MyCard.vue'
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <MyCard
      title="Мужские кроссовки Nike cosmos"
      img-url="/sneakers/sneakers-1.jpg"
      :price="1280"
      :isAdded="true"
      :isFavorite="true"
    />
  </div>
</template>
