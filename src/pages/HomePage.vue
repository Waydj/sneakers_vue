<script setup>
import MyCardList from '../components/MyCardList.vue'
import { inject } from 'vue'
import debounce from 'lodash.debounce'

const items = inject('items')
const filters = inject('filters')

const onChangeSelect = (e) => (filters.value.sortBy = e.target.value)
const onSearch = debounce((e) => (filters.value.searchQuery = e.target.value), 300)
</script>

<template>
  <div class="flex items-center justify-between">
    <h2 class="mb-8 text-3xl font-bold">Все кроссовки</h2>

    <div class="flex gap-4">
      <select @change="onChangeSelect" class="rounded-md border px-3 py-2 outline-none">
        <option value="title">По названию</option>
        <option value="price">По цене (дешевле)</option>
        <option value="-price">По цене (дороже)</option>
      </select>

      <div class="relative">
        <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
        <input
          @input="onSearch"
          type="search"
          placeholder="Поиск..."
          class="rounded-md border py-2 pl-11 pr-4 outline-none focus:border-slate-500"
        />
      </div>
    </div>
  </div>

  <MyCardList :items="items" />
</template>
