<script setup>
import { inject, computed } from 'vue'
import ky from 'ky'
import MyCartItemList from './MyCartItemList.vue'
import MyDrawerHead from './MyDrawerHead.vue'
import MyInfoBlock from './MyInfoBlock.vue'

const items = inject('items')
const cartItems = inject('cartItems')

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round(totalPrice.value * 0.05))

const emit = defineEmits(['onCartOpen'])

const onCartOpenHandler = () => {
  emit('onCartOpen')
}

const createOrder = async () => {
  try {
    const url = `https://d79b62e8a63cb906.mokky.dev/orders`
    const data = await ky
      .post(url, { json: { items: cartItems.value, totalPrice: totalPrice.value } })
      .json()

    cartItems.value = []
    items.value = items.value.map((item) => ({ ...item, isAdded: false }))
    return data
  } catch (error) {
    console.error(error)
  }
}
</script>

<template>
  <div class="fixed left-0 top-0 z-10 h-full w-full bg-black/50"></div>
  <div class="fixed right-0 top-0 z-20 flex h-full w-96 flex-col bg-white p-8">
    <MyDrawerHead :onCartOpen="onCartOpenHandler" />

    <MyInfoBlock
      title="Корзина пустая"
      description="Добавьте хотя бы одну пару кроссовок"
      img="/package-icon.png"
      v-if="!totalPrice"
    />

    <div v-else class="flex flex-1 flex-col">
      <MyCartItemList />

      <div class="my-7 flex flex-col gap-4">
        <div class="flex">
          <p>Итого:</p>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} руб.</b>
        </div>
        <div class="flex">
          <p>Налог:</p>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} руб.</b>
        </div>
        <button
          @click="createOrder"
          :disabled="totalPrice ? false : true"
          class="w-full cursor-pointer rounded-xl bg-lime-500 p-4 text-white transition hover:bg-lime-600 active:bg-lime-700 disabled:cursor-not-allowed disabled:bg-slate-400"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
