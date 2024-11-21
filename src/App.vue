<script setup>
import TheHeader from './components/TheHeader.vue'
import CardList from '@/components/CardList.vue'
import TheDrawer from '@/components/TheDrawer.vue'
import { onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

const items = ref([])

const filter = reactive({
  sortBy: '',
  searchQuery: '',
})

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filter.sortBy,
    }

    if (filter.searchQuery) {
      params.title = `*${filter.searchQuery}*`
    }

    const { data } = await axios.get('https://04cb287a5b978723.mokky.dev/items', {
      params,
    })

    items.value = data.map((item) => ({
      ...item,
      isFavorite: false,
      isAdded: false,
    }))
  } catch (error) {
    console.log(error)
  }
}

const onChangeSort = (e) => {
  filter.sortBy = e.target.value
}

const onChangeSearch = (e) => {
  filter.searchQuery = e.target.value
}

onMounted(fetchItems)

watch(filter, fetchItems)
</script>

<template>
  <TheDrawer v-if="false" />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <TheHeader />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Top Running Shoes</h2>

        <div class="flex gap-4">
          <select @change="onChangeSort" class="py-2 px-3 border rounded-md outline-none">
            <option value="">Recommended</option>
            <option value="title">Name A-Z</option>
            <option value="-title">Name Z-A</option>
            <option value="price">Low Price</option>
            <option value="-price">Height Price</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" />
            <input
              @input="onChangeSearch"
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Search..."
            />
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
