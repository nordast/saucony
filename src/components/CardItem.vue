<script setup>
import { onMounted, ref } from 'vue'

const props = defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isAdded: Boolean,
})

const storageName = 'saucony-favorites'

const isFavorite = ref(false)

onMounted(() => {
  const favorites = JSON.parse(localStorage.getItem(storageName)) || []
  isFavorite.value = favorites.includes(props.id)
})

function add() {
  let favorites = JSON.parse(localStorage.getItem(storageName)) || []

  if (!favorites.includes(props.id)) {
    favorites.push(props.id)
  }

  localStorage.setItem(storageName, JSON.stringify(favorites))
  isFavorite.value = true
}

function remove() {
  let favorites = JSON.parse(localStorage.getItem(storageName)) || []

  const newFavorites = favorites.filter((item) => item !== props.id)

  localStorage.setItem(storageName, JSON.stringify(newFavorites))
  isFavorite.value = false
}
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:shadow-md"
  >
    <img
      @click="isFavorite ? remove() : add()"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like 1"
      class="absolute top-8 left-8"
    />

    <img :src="imageUrl" :alt="title" />

    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>${{ price }}</b>
      </div>

      <img :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" />
    </div>
  </div>
</template>
