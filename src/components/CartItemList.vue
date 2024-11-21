<script setup>
import CartItem from './CartItem.vue'

const item = {
  id: 1,
  title: 'Ride 17',
  imageUrl: '/images/shoes-1.jpg',
  price: 1200,
}
</script>

<template>
  <div class="flex flex-col flex-1 gap-4 justify-between">
    <CartItem :id="item.id" :title="item.title" :price="item.price" :image-url="item.imageUrl" />
  </div>
</template>
