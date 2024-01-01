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
      <div class="flex items-center justify-between">
        <h2 class="mb-8 text-3xl font-bold">Все кроссовки</h2>

        <div class="flex gap-4">
          <select class="rounded-md border px-3 py-2 outline-none">
            <option value="">По названию</option>
            <option value="">По цене (дешевле)</option>
            <option value="">По цене (дороже)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
            <input
              type="search"
              placeholder="Поиск..."
              class="rounded-md border py-2 pl-11 pr-4 outline-none focus:border-slate-500"
            />
          </div>
        </div>
      </div>

      <MyCardList />
    </div>
  </div>
</template>

<style scoped></style>
