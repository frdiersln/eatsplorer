<script setup>
import { ref } from 'vue'
import { ingredients } from "../../stores/IngredientsStore.js"

let selectedIngredients = ref([])
let visibleIngredients = ref([])
let loading = ref(false)

const search = query => { //search with loading
  if (query !== '') {
    loading.value = true
    setTimeout(() => {
      visibleIngredients.value = ingredients.filter(item => item.label.toLowerCase().includes(query.toLowerCase()))
      loading.value = false
    }, 2000)
  } else {
    visibleIngredients.value = ingredients.slice(0, 26) // Show first 26 ingredients when search query is empty
  }
}
</script>

<template>
  <el-select
    v-model="selectedIngredients"
    multiple
    filterable
    placeholder="Select Ingredients That You Have"
    style="width: 66%"
    remote
    :remote-method="search"
    :loading="loading"
  >
    <el-option
      v-for="ingredient in visibleIngredients"
      :key="ingredient.value"
      :label="ingredient.label"
      :value="ingredient.value"
    />
  </el-select>
</template>