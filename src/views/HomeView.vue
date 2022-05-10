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
  newRecipe.value.slug = newRecipe.value.title.toLowerCase().replace(/\s/g, '-');

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

    <div class="add-recipe-popup" v-if="popupOpen">
      <div class="popup-content">
        <h2>Add new recipe</h2>

        <form @submit.prevent="addNewRecipe">
          <div class="group">
            <label>Title</label>
            <input type="text" v-model="newRecipe.title">
          </div>

          <div class="group">
            <label>Description</label>
            <textarea v-model="newRecipe.description"></textarea>
          </div>

          <div class="group">
            <label>Ingredients</label>
            <div class="ingredient" v-for="i in newRecipe.ingredientRows" :key="i">
              <input type="text" v-model="newRecipe.ingredients[i - 1]">
            </div>
            <button type="button" @click="addNewIngredient">Add Ingredient</button>
          </div>

          <div class="group">
            <label>Method</label>
            <div class="method" v-for="i in newRecipe.methodRows" :key="i">
              <textarea v-model="newRecipe.method[i - 1]"></textarea>
            </div>
            <button type="button" @click="addNewStep">Add step</button>
          </div>

          <button type="submit">Add Recipe</button>
          <button type="button" @click="togglePopup">Close</button>
          
        </form>
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
}
.add-recipe-popup {
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

.add-recipe-popup .popup-content {
  background-color: #081c33;
  padding: 2rem;
  border-radius: 1rem;
  width: 100%;
  min-width: 768px;
}

.popup-content h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}

.popup-content .group {
  margin-bottom: 1rem;
}

.popup-content .group label {
  display: block;
  margin-bottom: 0.5rem;
}

.popup-content .group input,
.popup-content .group textarea {
  display: block;
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 1rem;
}

.popup-content .group textarea {
  height: 100px;
  resize: none;
}

.popup-content button[type="submit"] {
  margin-right: 1rem;
}
</style>
