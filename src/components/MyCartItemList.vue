<script setup>
import { inject } from 'vue'
import MyCartItem from './MyCartItem.vue'

const cartItems = inject('cartItems')
const addToCart = inject('addToCart')
</script>

<template>
  <div class="flex flex-1 flex-col gap-4">
    <MyCartItem
      v-for="item in cartItems"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :onAddToCart="() => addToCart(item)"
    />
  </div>
</template>
