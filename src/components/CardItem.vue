<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
})
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:shadow-md"
  >
    <img
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
