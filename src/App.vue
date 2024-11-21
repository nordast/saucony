<script setup>
import TheHeader from './components/TheHeader.vue'
import AppCardList from '@/components/Card/AppCardList.vue'
</script>

<template>
  <div class="bg-white w-3/5 m-auto rounded-xl shadow-xl shadow-grey-200 mt-20">
    <TheHeader />

    <div class="p-10">
      <h1 class="text-3xl font-bold mb-8">Top Running Shoes</h1>

      <AppCardList />
    </div>
  </div>
</template>

<style scoped></style>
