<script setup>
import CardList from '@/components/CardList.vue'
import { inject, onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])

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

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSort = (e) => {
  filter.sortBy = e.target.value
}

const onChangeSearch = (e) => {
  filter.searchQuery = e.target.value
}

onMounted(async () => {
  const localCart = localStorage.getItem('saucony-cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()

  // items.value = items.value.map((item) => ({
  //   ...item,
  //   isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  // }))
})

watch(filter, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
})
</script>

<template>
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
</template>

<style scoped></style>
