<script setup>
import { ref, onMounted } from 'vue'
import ky from 'ky'
import MyCardList from '@/components/MyCardList.vue'
import { SERVER_URL } from '../services/api'

const items = ref([])

const fetchFavoritesData = async () => {
  try {
    const url = `${SERVER_URL}/favorites?_relations=items`

    const response = await ky.get(url)
    const data = await response.json()

    items.value = data.map((item) => item.item)
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
