<script setup>
import TheHeader from './components/TheHeader.vue'
import CardList from '@/components/CardList.vue'
import TheDrawer from '@/components/TheDrawer.vue'
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'
import TheFooter from '@/components/TheFooter.vue'

const items = ref([])
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

const filter = reactive({
  sortBy: '',
  searchQuery: '',
})

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filter.sortBy,
    }

    if (filter.searchQuery) {
      params.title = `*${filter.searchQuery}*`
    }

    const { data } = await axios.get('https://04cb287a5b978723.mokky.dev/items', {
      params,
    })

    items.value = data.map((item) => ({
      ...item,
      isFavorite: false,
      isAdded: false,
    }))
  } catch (error) {
    console.log(error)
  }
}

const onChangeSort = (e) => {
  filter.sortBy = e.target.value
}

const onChangeSearch = (e) => {
  filter.searchQuery = e.target.value
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
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

onMounted(async () => {
  const localCart = localStorage.getItem('saucony-cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  }))
})

watch(filter, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
})

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
    <div class="flex justify-between items-center">
      <h2 class="text-3xl font-bold mb-8">Top Running Shoes</h2>

      <div class="flex gap-4">
        <select @change="onChangeSort" class="py-2 px-3 border rounded-md outline-none">
          <option value="">Recommended</option>
          <option value="title">Name A-Z</option>
          <option value="-title">Name Z-A</option>
          <option value="price">Low Price</option>
          <option value="-price">Height Price</option>
        </select>

        <div class="relative">
          <img class="absolute left-4 top-3" src="/search.svg" />
          <input
            @input="onChangeSearch"
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            type="text"
            placeholder="Search..."
          />
        </div>
      </div>
    </div>

    <div class="mt-10">
      <CardList :items="items" @add-to-cart="onClickAddPlus" />
    </div>
  </div>

  <TheFooter />
</template>

<style scoped></style>
