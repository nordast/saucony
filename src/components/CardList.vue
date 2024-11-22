<script setup>
import CardItem from './CardItem.vue'

defineProps({
  items: Array,
})

const emit = defineEmits(['addToCart'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <CardItem
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :image-url-hover="item.imageUrlHover"
      :price="item.price"
      :is-added="item.isAdded"
      :on-click-add="() => emit('addToCart', item)"
    />
  </div>
</template>
