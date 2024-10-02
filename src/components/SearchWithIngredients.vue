<script setup>
import { ref, computed } from 'vue'
import { ingredients } from "../../stores/IngredientsStore.js"
import axios from 'axios';


var selectedIngredients = ref([])
var visibleIngredients = ref([])
var loading = ref(false)
const hasSelectedIngredients = computed(() => {
  return selectedIngredients.value.length > 0;
})

function searchIngredientsPhase(){
  console.log('searchIngredientsPhase')
  document.querySelector('.container').classList.add('container_searchPhase')
}

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
  let apiKey = process.env.SPOONACULAR_API_KEY;
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
<div class="container">
  <div class="logoWrapper">
    <img src="@/assets/logo.svg" alt="logo" />
  </div>
  <div class="searchBarWrapper">
    <el-select
    v-model="selectedIngredients"
    multiple
    filterable
    placeholder="Select Ingredients That You Have"
    remote
    :remote-method="search"
    :loading="loading"
    @click="searchIngredientsPhase()"
  >
    <el-option
      v-for="ingredient in visibleIngredients"
      :key="ingredient.value"
      :label="ingredient.label"
      :value="ingredient.value"
    />
  </el-select>
  <el-button
    type="outline-primary"
    @click="searchRecipes()"
    :disabled="!hasSelectedIngredients">
    Search
  </el-button>
  </div>
</div>
</template>

<style lang="scss">
.container{
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
  height: 100vh;
  min-width: 766px;
  .searchBarWrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    min-width: 341px;
    margin-left: -66px;
    .el-button {
      position: relative;
      right: 4px;
      border-radius: 0px 4px 4px 0px;
      height: 100%;
    }
  }  
}


.container_searchPhase{
  flex-direction: column-reverse;
  justify-content: flex-end;
  .searchBarWrapper {
    width: 66%;
    margin: 89px 0;
  }
}
</style>