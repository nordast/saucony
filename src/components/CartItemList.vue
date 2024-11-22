<script setup>
import CartItem from './CartItem.vue'
import { inject } from 'vue'

const item = {
  id: 1,
  title: 'Ride 17',
  imageUrl: '/images/shoes-1.jpg',
  price: 1200,
}

const { cart, removeFromCart } = inject('cart')
</script>

<template>
  <div class="flex flex-col flex-1 gap-4 justify-between">
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :item="item"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>
