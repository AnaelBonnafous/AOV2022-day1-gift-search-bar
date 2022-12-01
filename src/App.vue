<script setup>
import { ref, watch } from 'vue'

const searchTerm = ref('')
const products = ref([])
const loading = ref(false)

const findProducts = async term => {
  loading.value = true
  try {
    const { products: data } = await (await fetch(`https://dummyjson.com/products/search?q=${term}&limit=5`)).json()
    products.value = data
    loading.value = false
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
    <template v-if="loading">
      <svg class="spinner" viewBox="0 0 50 50">
        <circle class="path" cx="25" cy="25" r="20" fill="none" stroke-width="5"></circle>
      </svg>
    </template>
    <template v-else>
      <ul v-if="products.length" class="list-disc">
        <li v-for="product in products">{{ product.title }}: {{ product.price }}$</li>
      </ul>
      <p v-else>No products found...</p>
    </template>
  </div>
</template>

<style scoped>
.spinner {
  width: 50px;
  height: 50px;
  animation: rotate 2s linear infinite;
}

.spinner .path {
  stroke: black;
  stroke-linecap: round;
  animation: dash 1.5s ease-in-out infinite;
}

@keyframes rotate {
  100% {
    transform: rotate(360deg);
  }
}

@keyframes dash {
  0% {
    stroke-dasharray: 1, 150;
    stroke-dashoffset: 0;
  }
  50% {
    stroke-dasharray: 90, 150;
    stroke-dashoffset: -35;
  }
  100% {
    stroke-dasharray: 90, 150;
    stroke-dashoffset: -124;
  }
}
</style>
