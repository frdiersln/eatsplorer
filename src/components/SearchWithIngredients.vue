<script setup>
import { ref } from 'vue'
import { ingredients } from "../../stores/IngredientsStore.js"
import axios from 'axios';


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

function searchRecipes() {
  // Get selected ingredient labels from selected ingredient values
  var selectedIngredientLabels = [];
  for (let ingredientValue of selectedIngredients.value) {
    selectedIngredientLabels.push(ingredients.filter(item => item.value === ingredientValue)[0].label)
  }
  console.log(selectedIngredientLabels)
  // Call API to search recipes with selected ingredients
  let apiKey = process.env.VUE_APP_SPOONACULAR_API_KEY;
  let selectedIngredientLabelsString = selectedIngredientLabels.join(',')

  axios.get('https://api.spoonacular.com/recipes/findByIngredients?ingredients=' + selectedIngredientLabelsString + '&number=2&apiKey=' + apiKey)
    .then(response => {
      console.log(response.data)
    })
    .catch(error => {
      console.log(error)
    })
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

  <el-button type="primary" @click="searchRecipes()">
    Search
  </el-button>
</template>