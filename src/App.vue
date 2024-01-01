<script setup>
import { onMounted, ref, watch } from 'vue'
import ky from 'ky'
import MyCardList from './components/MyCardList.vue'
import MyDrawer from './components/MyDrawer.vue'
import MyHeader from '/src/components/MyHeader.vue'

const items = ref([])
const filters = ref({
  searchQuery: '',
  sortBy: 'title'
})

const onChangeSelect = (e) => (filters.value.sortBy = e.target.value)
const onSearch = (e) => (filters.value.searchQuery = e.target.value)

const fetchData = async () => {
  try {
    const url = `https://d79b62e8a63cb906.mokky.dev/items`

    const params = {
      sortBy: filters.value.sortBy
    }

    if (filters.value.searchQuery) {
      params.title = `*${filters.value.searchQuery}*`
    }

    const response = await ky.get(url, { searchParams: params })
    const data = await response.json()

    items.value = data
  } catch (error) {
    console.error(error)
  }
}

onMounted(fetchData)
watch(filters.value, fetchData)
</script>

<template>
  <!-- <MyDrawer /> -->

  <div class="m-auto my-10 w-4/5 rounded-xl bg-white shadow-xl">
    <MyHeader />

    <div class="p-10">
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
    </div>
  </div>
</template>

<style scoped></style>
