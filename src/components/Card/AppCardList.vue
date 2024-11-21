<script setup>
import AppCard from './AppCard.vue'

const item = {
  title: 'Ride 17',
  imageUrl: '/images/shoes-1.jpg',
  price: 1200,
  isFavorite: false,
  isAdded: false,
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <AppCard
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
