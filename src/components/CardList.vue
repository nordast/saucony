<script setup>
import CardItem from './CardItem.vue'

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
    <CardItem
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
