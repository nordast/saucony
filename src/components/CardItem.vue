<script setup>
import { onMounted, ref } from 'vue'

const props = defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  imageUrlHover: String,
  price: Number,
  isAdded: Boolean,
  onClickAdd: Function,
})

const storageName = 'saucony-favorites'

const isFavorite = ref(false)
const imageSrc = ref(props.imageUrl)

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

  favorites.splice(favorites.indexOf(props.id), 1)
  localStorage.setItem(storageName, JSON.stringify(favorites))

  isFavorite.value = false
}
</script>

<template>
  <div
    @mouseenter="imageSrc = props.imageUrlHover"
    @mouseleave="imageSrc = props.imageUrl"
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:shadow-md"
  >
    <img
      @click="isFavorite ? remove() : add()"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Wishlist"
      title="Wishlist"
      class="absolute top-8 left-8"
    />

    <img :src="imageSrc" :alt="title" />

    <p class="mt-2 text-lg text-semibold">{{ title }}</p>

    <div class="flex justify-between items-center mt-2">
      <div class="flex flex-col">
        <span class="text-slate-400">${{ price }}.00</span>
      </div>

      <img
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="Add to Cart"
        title="Add to Cart"
      />
    </div>
  </div>
</template>
