<script setup>
import { ref, provide, watch, computed } from 'vue'
import TheHeader from './components/TheHeader.vue'
import TheDrawer from './components/TheDrawer.vue'
import TheFooter from '@/components/TheFooter.vue'
import { LOCAL_STORAGE_KEY_PREFIX } from '@/constants.js'

const cart = ref([])
const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem(LOCAL_STORAGE_KEY_PREFIX + 'cart', JSON.stringify(cart.value))
  },
  { deep: true },
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
})
</script>

<template>
  <TheDrawer v-if="drawerOpen" :total-price="totalPrice" />
  <TheHeader @open-drawer="openDrawer" :total-price="totalPrice" />

  <div class="p-10">
    <RouterView />
  </div>

  <TheFooter />
</template>

<style scoped></style>
