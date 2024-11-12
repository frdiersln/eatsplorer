<!-- eslint-disable no-unused-vars -->
<script setup>
import { ref, computed } from 'vue'
import { ingredients } from "../../stores/IngredientsStore.js"
import axios from 'axios';
import ResultCard from './resultCard.vue'
import recipePage from './recipePage.vue'

// Call API to search recipes with selected ingredients
const apiKey = process.env.VUE_APP_SPOONACULAR_API_KEY; // MAKE SURE YOU HAVE spoonacular API KEY IN YOUR .env(create it in root folder) FILE
var selectedIngredients = ref([])
var visibleIngredients = ref([])
var loading = ref(false)
var Resultsloading = ref(false)
const hasSelectedIngredients = computed(() => {
  return selectedIngredients.value.length > 0;
})
var results = ref([])


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
  Resultsloading.value = true
  results.value = [] // Clear previous results
  // Get selected ingredient labels from selected ingredient values
  var selectedIngredientLabels = [];
  for (let ingredientValue of selectedIngredients.value) {
    selectedIngredientLabels.push(ingredients.filter(item => item.value === ingredientValue)[0].label)
  }
  console.log(selectedIngredientLabels)
  let selectedIngredientLabelsString = selectedIngredientLabels.join(',')

  axios.get('https://api.spoonacular.com/recipes/findByIngredients?ingredients=' + selectedIngredientLabelsString + '&apiKey=' + apiKey)
    .then(response => {
      for (let recipe of response.data) {
        // Create and push to results a result card for each recipe
        results.value.push({
          id: recipe.id,
          image: recipe.image,
          title: recipe.title,
          likes: recipe.likes,
          missedIngredientsCount: recipe.missedIngredients.length,
          unusedIngredientsCount: recipe.unusedIngredients.length
        })
        Resultsloading.value = false
      }
    })
    .catch(error => {
      console.log(error)
      Resultsloading.value = false
    })
}

function showRecipeDetails(recipeId) {
  console.log('showRecipeDetails')
  // Call API to get recipe details
  axios.get('https://api.spoonacular.com/recipes/' + recipeId + '/information?includeNutrition=false&apiKey=' + apiKey)
    .then(response => {
      var recipeData = {
        title: response.data.title,
        image: response.data.image,
        summary: response.data.summary,
        servings: response.data.servings,
        readyInMinutes: response.data.readyInMinutes,
        healthScore: response.data.healthScore,
        sourceName: response.data.sourceName,
        sourceUrl: response.data.sourceUrl
      }
      let recipePage = document.getElementById('recipePage')
      recipePage.querySelector('#title').textContent = recipeData.title
      recipePage.querySelector('#image').src = recipeData.image
      recipePage.querySelector('#image').alt = recipeData.title
      recipePage.querySelector('#summary').innerHTML = recipeData.summary
      recipePage.querySelector('#servings').textContent = recipeData.servings
      recipePage.querySelector('#readyIn').textContent = recipeData.readyInMinutes + ' minutes'
      recipePage.querySelector('#healthScore').textContent = recipeData.healthScore
      recipePage.querySelector('#source').textContent = recipeData.sourceName
      recipePage.querySelector('#source').href = recipeData.sourceUrl
      recipePage.style.display = 'block'
      document.querySelector('.backDrop').style.display = 'block'
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
<div class="resultWrapper">
  <iframe v-if="Resultsloading" class="loading" src="https://lottie.host/embed/dba9ec49-798b-406b-9f4c-39d0dcc56224/dt6MSPB6ti.json"></iframe>
  <ResultCard
    @click="showRecipeDetails(result.id)"
    v-for="result in results"
    :key="result.title"
    :image="result.image"
    :title="result.title"
    :likes="result.likes"
    :missedIngredientsCount="result.missedIngredientsCount"
    :unusedIngredientsCount="result.unusedIngredientsCount"
  />
</div>

<recipePage/>
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
  height: fit-content;
  .logoWrapper {
    max-height: 66px;
    margin-right: 22px;
    img{
      max-height: 66px;
    }
  }
  .searchBarWrapper {
    width: 66%;
    margin: 89px 0;
  } 
}

.resultWrapper{
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
  gap: 2rem;
  padding: 1rem 16rem;
  .loading{
    border: none;
    margin: 0 auto;
  }
}
</style>