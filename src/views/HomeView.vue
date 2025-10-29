<script setup lang="ts">
import api from '@/api';
import AppCard from '@/components/AppCard.vue';
import AppLayout from '@/components/AppLayout.vue';
import type { Recipe } from '@/types/Recipe';
import { ref } from 'vue';

const recipes = ref<Recipe[] | null>(null);
const isLoading = ref<boolean>(true);

async function getRecipes() {
  recipes.value = await api.recipeService.getRecipes();
  isLoading.value = false;
}

getRecipes();
</script>

<template>
  <AppLayout class="recipes-list">
    <h1>Recipes</h1>

    <span v-if="isLoading">Loading recipes...</span>

    <RouterLink
      v-else
      v-for="recipe in recipes"
      :key="recipe.id"
      :to="{ name: 'recipe', params: { id: recipe.id } }"
      class="recipes-list__link"
    >
      <AppCard>
        <h2>{{ recipe.title }}</h2>
      </AppCard>
    </RouterLink>
  </AppLayout>
</template>

<style lang="scss" scoped>
.recipes-list {
  display: flex;
  flex-direction: column;
  gap: 2rem;

  &__link {
    text-decoration: none;
    color: var(--rt-color-stone-900);
    width: 100%;
    max-width: 44rem;

    &:hover {
      color: var(--rt-color-rose-800);
    }
  }
}
</style>
