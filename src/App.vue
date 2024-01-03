<script setup>
import { computed, onMounted, provide, ref, watch } from 'vue'
import ky from 'ky'
import MyCardList from './components/MyCardList.vue'
import MyDrawer from './components/MyDrawer.vue'
import MyHeader from '/src/components/MyHeader.vue'

const items = ref([])
const cartItems = ref([])

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round(totalPrice.value * 0.05))

const isCartOpen = ref(false)
const filters = ref({
  searchQuery: '',
  sortBy: 'title'
})

const onCartOpen = () => {
  isCartOpen.value = !isCartOpen.value
}

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
    const url = `https://d79b62e8a63cb906.mokky.dev/favorites`

    const response = await ky.get(url)
    const data = await response.json()

    items.value = items.value.map((item) => {
      const favorite = data.find((favorite) => favorite.parentId === item.id)

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
      const url = `https://d79b62e8a63cb906.mokky.dev/favorites`
      const data = await ky.post(url, { json: { ...item, parentId: item.id } }).json()

      item.isFavorite = true
      item.favoriteId = data.id
    } else {
      const url = `https://d79b62e8a63cb906.mokky.dev/favorites/${item.favoriteId}`
      await ky.delete(url)

      item.isFavorite = false
      item.favoriteId = null
    }
  } catch (error) {
    console.error(error)
  }
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
})
watch(filters.value, async () => {
  await fetchData()
  await fetchFavoritesData()
})

// Test provide / inject
provide('addToFavorites', addToFavorites)
provide('addToCart', addToCart)
provide('cartItems', cartItems)
</script>

<template>
  <MyDrawer
    v-if="isCartOpen"
    @onCartOpen="onCartOpen"
    @createOrder="createOrder"
    :totalPrice="totalPrice"
    :vatPrice="vatPrice"
  />

  <div class="m-auto my-10 w-4/5 rounded-xl bg-white shadow-xl">
    <MyHeader @onCartOpen="onCartOpen" :totalPrice="totalPrice" />

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
