<script setup>
import { ref } from 'vue';
import { useStore } from 'vuex';

const newRecipe = ref({
  title: '',
  description: '',
  ingredients: [],
  method: [],
  ingredientRows: 1,
  methodRows: 1
});
const popupOpen = ref(false);
const store = useStore();

function togglePopup() {
  popupOpen.value = !popupOpen.value;
}

function addNewIngredient() {
  newRecipe.value.ingredientRows++;
}

function addNewStep() {
  newRecipe.value.methodRows++;
}

function addNewRecipe() {
  newRecipe.value.slug = newRecipe.value
  .title.toLowerCase()
  .replace(/\s/g, '-');

  if(newRecipe.value.slug === '') {
    alert('Please enter a title');
    return;
  }

  store.commit('ADD_RECIPE', { ...newRecipe.value });

  newRecipe.value = {
    title: '',
    description: '',
    ingredients: [],
    method: [],
    ingredientRows: 1,
    methodRows: 1
  };

  togglePopup();
}
</script>
<template>
  <div class="home-view">
    <h1 class="home-view__title">My Recipes</h1>
    <el-button type="primary" @click="togglePopup">Add new Recipe</el-button>
    <div class="home-view__recipes">
      <div class="home-view__recipe-card" v-for="recipe in $store.state.recipes" :key="recipe.slug">
        <h2 class="home-view__recipe-title">{{ recipe.title }}</h2>
        <p class="home-view__recipe-paragraph">{{ recipe.description }}</p>
        <router-link :to="`/recipe/${recipe.slug}`">
          <el-button>View Recipe</el-button>
        </router-link>
      </div>
    </div>

    <div class="home-view__popup" v-if="popupOpen">
      <div class="home-view__popup-content">
        <h2 class="home-view__popup-title">Add new recipe</h2>

        <el-form @submit.prevent="addNewRecipe">
          <div class="home-view__popup-group">
            <label>Title</label>
            <input class="home-view__popup-input" type="text" v-model="newRecipe.title">
          </div>

          <div class="home-view__popup-group">
            <label class="home-view__popup-label">Description</label>
            <textarea class="home-view__popup-input home-view__popup-input--textarea" v-model="newRecipe.description"></textarea>
          </div>

          <div class="home-view__popup-group">
            <label class="home-view__popup-label">Ingredients</label>
            <div class="home-view__popup-ingredient" v-for="i in newRecipe.ingredientRows" :key="i">
              <input class="home-view__popup-input" type="text" v-model="newRecipe.ingredients[i - 1]">
            </div>
            <el-button type="button" @click="addNewIngredient">Add Ingredient</el-button>
          </div>

          <div class="home-view__popup-group">
            <label class="home-view__popup-label">Method</label>
            <div class="home-view__popup-method" v-for="i in newRecipe.methodRows" :key="i">
              <textarea class="home-view__popup-input home-view__popup-input--textarea" v-model="newRecipe.method[i - 1]"></textarea>
            </div>
            <el-button type="button" @click="addNewStep">Add step</el-button>
          </div>

          <el-button type="primary" @click="addNewRecipe">Add Recipe</el-button>
          <el-button type="danger" @click="togglePopup">Close</el-button>
          
        </el-form>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.home-view {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;

  &__title {
    font-size: 3rem;
    margin-bottom: 32px;
  }

  &__recipes {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }

  &__recipe-card {
    padding: 1rem;
    border-radius: 5px;
    margin: 1rem;
    background-color: #081c33;
  }

  &__recipe-title {
    font-size: 2rem;
    margin-bottom: 1rem;
  }

  &__recipe-paragraph {
    font-size: 1.125rem;
    line-height: 1.4;
    margin-bottom: 1rem;
  }

  &__popup{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  &__popup-content {
    background-color: #081c33;
    padding: 2rem;
    border-radius: 1rem;
    width: 100%;
    min-width: 768px;
  }

  &__popup-title {
    font-size: 2rem;
    margin-bottom: 1rem;
  }

  &__popup-group {
    margin-bottom: 1rem;
  }

  &__popup-label {
    display: block;
    margin-bottom: 0.5rem;
  }

  &__popup-input{
    display: block;
    width: 100%;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-bottom: 1rem;
  }

  &__popup-input--textarea{
    height: 100px;
    resize: none;
  }
}
</style>
