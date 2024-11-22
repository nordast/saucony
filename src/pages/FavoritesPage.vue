<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import CardList from '@/components/CardList.vue'
import { API_STORAGE_URL } from '@/constants.js'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(`${API_STORAGE_URL}/favorites?_relations=items`)
    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Wishlist</h2>

  <CardList v-if="!!favorites.length" :items="favorites" is-favorites />

  <div v-else class="my-20">No products were found</div>
</template>
