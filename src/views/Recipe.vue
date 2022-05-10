<template>
    <div class="recipe">
        <router-link to="/">&lt; Back</router-link>
        <h1>{{ recipe.title }}</h1>
        <p class="recipe__description">{{ recipe.description }}</p>
        <hr class="recipe__hr">
        <div class="recipe__ingredients">
            <h3 class="recipe__title">Ingredients</h3>
            <ul class="recipe__ingredients-ul">
                <li class="recipe__ingredients-ul-li" v-for="(ingredient, i) in recipe.ingredients" :key="i">
                    {{ingredient}}
                </li>
            </ul>
        </div>
        <div class="recipe__method">
            <h3 class="recipe__title">Method</h3>
            <ol class="recipe__ingredients-ol">
                <li class="recipe__ingredients-ol-li" v-for="(step, i) in recipe.method" :key="i">
                    <span v-html="cleanText(step)"></span>
                </li>
            </ol>
        </div>
    </div>
</template>

<script>
export default {
    computed: {
        recipe() {
            return this.$store.state.recipes.find(recipe => recipe.slug === this.$route.params.slug);
        }
    },
    methods: {
        cleanText(text) {
            return text.replace(/\n/g, '<br>');
        }
    }
}
</script>

<style lang="scss">
.recipe {
    padding: 1rem;
    max-width: 768px;
    margin: 0 auto;

    &__description {
        font-size: 1.125rem;
        line-height: 1.4;
        margin-bottom: 1rem;
    }
    
    &__ingredients {
        padding: 1rem;
        background-color: #081c33;
        border-radius: 0.5rem;
    }

    &__hr {
        margin-bottom: 1rem;
    }

    &__title {
        margin-bottom: 1rem;
    }

    &__ingredients-ul-li {
        list-style-position: inside;
        line-height: 1.4;
        margin-bottom: 1rem;
    }

    &__ingredients-ol-li {
        margin-bottom: 2rem;
        padding-bottom: 1rem;
        list-style-position: inside;
        border-bottom: 1px solid #eee;
    }
}
</style>