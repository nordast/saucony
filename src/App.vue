<script setup>
import TheHeader from './components/TheHeader.vue'
import TheDrawer from '@/components/TheDrawer.vue'
import { computed, provide, ref, watch } from 'vue'
import axios from 'axios'
import TheFooter from '@/components/TheFooter.vue'
import HomePage from '@/pages/HomePage.vue'

const cart = ref([])
const drawerOpen = ref(false)
const isCreatingOrder = ref(false)

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

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://04cb287a5b978723.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value,
    })

    cart.value = []

    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

watch(
  cart,
  () => {
    localStorage.setItem('saucony-cart', JSON.stringify(cart.value))
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
  <TheDrawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    @create-order="createOrder"
    :is-creating-order="isCreatingOrder"
  />

  <TheHeader @open-drawer="openDrawer" :total-price="totalPrice" />

  <div class="p-10">
    <RouterView />
  </div>

  <TheFooter />
</template>

<style scoped></style>
