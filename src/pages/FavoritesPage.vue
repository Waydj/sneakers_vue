<script setup>
import { ref, onMounted } from 'vue'
import ky from 'ky'
import MyCardList from '@/components/MyCardList.vue'

const items = ref([])

const fetchFavoritesData = async () => {
  try {
    const url = `https://d79b62e8a63cb906.mokky.dev/favorites`

    const response = await ky.get(url)
    const data = await response.json()

    items.value = data
  } catch (error) {
    console.error(error)
  }
}

onMounted(async () => {
  await fetchFavoritesData()
})
</script>

<template>
  <div class="flex items-center justify-between">
    <h2 class="mb-8 text-3xl font-bold">Закладки</h2>
  </div>
  <MyCardList :items="items" :noButtons="true" />
</template>
