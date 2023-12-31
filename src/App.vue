<script setup>
import { onMounted, provide, ref, watch } from 'vue'
import { RouterView } from 'vue-router'
import ky from 'ky'
import MyDrawer from './components/MyDrawer.vue'
import MyHeader from '/src/components/MyHeader.vue'
import { SERVER_URL } from './services/api'

const items = ref([])
const cartItems = ref([])

const isCartOpen = ref(false)
const filters = ref({
  searchQuery: '',
  sortBy: 'title'
})

const onCartOpen = () => {
  isCartOpen.value = !isCartOpen.value
}

const fetchData = async () => {
  try {
    const url = `${SERVER_URL}/items`

    const params = {
      sortBy: filters.value.sortBy
    }

    if (filters.value.searchQuery) {
      params.title = `*${filters.value.searchQuery}*`
    }

    const response = await ky.get(url, { searchParams: params })
    const data = await response.json()

    items.value = data.map((item) => ({
      ...item,
      isFavorite: false,
      isAdded: false,
      favoriteId: null
    }))
  } catch (error) {
    console.error(error)
  }
}

const fetchFavoritesData = async () => {
  try {
    const url = `${SERVER_URL}/favorites`

    const response = await ky.get(url)
    const data = await response.json()

    items.value = items.value.map((item) => {
      const favorite = data.find((favorite) => favorite.item_id === item.id)

      if (!favorite) return item

      return { ...item, isFavorite: true, favoriteId: favorite.id }
    })
  } catch (error) {
    console.error(error)
  }
}

const addToFavorites = async (item) => {
  try {
    if (!item.isFavorite) {
      const url = `${SERVER_URL}/favorites`
      const data = await ky.post(url, { json: { item_id: item.id } }).json()

      item.isFavorite = true
      item.favoriteId = data.id
    } else {
      const url = `${SERVER_URL}/favorites/${item.favoriteId}`
      await ky.delete(url)

      item.isFavorite = false
      item.favoriteId = null
    }
  } catch (error) {
    console.error(error)
  }
}

const addToCart = (item) => {
  if (item.isAdded) {
    item.isAdded = false
    cartItems.value = cartItems.value.filter((cartItem) => cartItem.id !== item.id)
  } else {
    item.isAdded = true
    cartItems.value.push(item)
  }
}

onMounted(async () => {
  await fetchData()
  await fetchFavoritesData()
  cartItems.value = JSON.parse(localStorage.getItem('cartItems')) || []
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
})
watch(filters.value, async () => {
  await fetchData()
  await fetchFavoritesData()
})
watch(
  cartItems,
  () => {
    localStorage.setItem('cartItems', JSON.stringify(cartItems.value))
  },
  { deep: true }
)

// Test provide / inject
provide('addToFavorites', addToFavorites)
provide('addToCart', addToCart)
provide('cartItems', cartItems)
provide('filters', filters)
provide('items', items)
</script>

<template>
  <MyDrawer v-if="isCartOpen" @onCartOpen="onCartOpen" />

  <div class="m-auto my-10 w-4/5 rounded-xl bg-white shadow-xl">
    <MyHeader @onCartOpen="onCartOpen" />

    <div class="p-10">
      <RouterView />
    </div>
  </div>
</template>

<style scoped></style>
