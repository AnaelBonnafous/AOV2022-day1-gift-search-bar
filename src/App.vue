<script setup>
import { ref, watch } from 'vue'

const searchTerm = ref('')
const products = ref([])

const findProducts = async term => {
  try {
    const { products: data } = await (await fetch(`https://dummyjson.com/products/search?q=${term}&limit=5`)).json()
    products.value = data
  } catch (error) {
    alert('An error occurred while searching products. Please try again later.')
  }
}

watch(searchTerm, newTerm => {
  if (newTerm) {
    findProducts(newTerm)
  } else {
    products.value = []
  }
})
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <input type="text" class="p-2 border-2 border-gray-dark" v-model="searchTerm" placeholder="Start typing..." />
    <ul v-if="products.length" class="list-disc">
      <li v-for="product in products">{{ product.title }}: {{ product.price }}$</li>
    </ul>
    <p v-else>No products found...</p>
  </div>
</template>
