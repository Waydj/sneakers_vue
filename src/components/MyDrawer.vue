<script setup>
import MyCartItemList from './MyCartItemList.vue'
import MyDrawerHead from './MyDrawerHead.vue'
</script>

<template>
  <div class="fixed left-0 top-0 z-10 h-full w-full bg-black/50"></div>
  <div class="fixed right-0 top-0 z-20 flex h-full w-96 flex-col bg-white p-8">
    <MyDrawerHead />
    <MyCartItemList />

    <div class="my-7 flex flex-col gap-4">
      <div class="flex">
        <p>Итого:</p>
        <div class="flex-1 border-b border-dashed"></div>
        <b>1290 руб.</b>
      </div>
      <div class="flex">
        <p>Налог:</p>
        <div class="flex-1 border-b border-dashed"></div>
        <b>90 руб.</b>
      </div>
      <button
        class="w-full cursor-pointer rounded-xl bg-lime-500 p-4 text-white transition hover:bg-lime-600 active:bg-lime-700 disabled:cursor-not-allowed disabled:bg-slate-400"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
