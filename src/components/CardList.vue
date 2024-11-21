<script setup>
import CardItem from './CardItem.vue'

defineProps({
  items: Array,
})
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <CardItem
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :imageUrlHover="item.imageUrlHover"
      :price="item.price"
    />
  </div>
</template>
