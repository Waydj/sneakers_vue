<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['onCartOpen'])

const onCartOpenHandler = () => {
  emit('onCartOpen')
}
</script>

<template>
  <header class="flex justify-between border-b border-slate-400 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="/logo.png" alt="logo" class="w-10" />
      <div>
        <RouterLink to="/"><h2 class="text-xl font-bold uppercase">Vue Sneakers</h2></RouterLink>
        <p class="text-slate-500">Магазин лучших кроссовок</p>
      </div>
    </div>

    <ul class="flex items-center gap-10">
      <li
        @click="onCartOpenHandler"
        class="flex cursor-pointer items-center gap-3 text-slate-500 hover:text-black"
      >
        <img src="/cart.svg" alt="cart" />
        <b>{{ totalPrice }} руб.</b>
      </li>
      <RouterLink to="/favorites">
        <li class="flex cursor-pointer items-center gap-3 text-slate-500 hover:text-black">
          <img src="/heart.svg" alt="heart" />
          Закладки
        </li>
      </RouterLink>
      <li class="flex cursor-pointer items-center gap-3 text-slate-500 hover:text-black">
        <img src="/profile.svg" alt="profile" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>
