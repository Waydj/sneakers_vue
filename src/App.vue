<script setup>
import MyCardList from './components/MyCardList.vue'
import MyDrawer from './components/MyDrawer.vue'
import MyHeader from '/src/components/MyHeader.vue'
</script>

<template>
  <!-- <MyDrawer /> -->

  <div class="m-auto my-10 w-4/5 rounded-xl bg-white shadow-xl">
    <MyHeader />

    <div class="p-10">
      <h2 class="mb-8 text-3xl font-bold">Все кроссовки</h2>

      <MyCardList />
    </div>
  </div>
</template>

<style scoped></style>
