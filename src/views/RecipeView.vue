<script setup lang="ts">
  import AppLayout from '@/components/AppLayout.vue';
  import api from '@/api';
  import type { Recipe } from '@/types/Recipe';
  import { ref } from 'vue';
  import { useRoute } from 'vue-router';
  import RecipeTable from '@/components/RecipeTable.vue';
  import RecipeList from '@/components/RecipeList.vue';
  import RecipeCallout from '@/components/RecipeCallout.vue';
  import AppCard from '@/components/AppCard.vue';

  const route = useRoute();

  const recipe = ref<Recipe | null>(null);
  const isLoading = ref<boolean>(true);

  async function getRecipe() {
    recipe.value = await api.recipeService.getRecipeById(
      <string>route.params.id,
    );

    isLoading.value = false;
  }

  getRecipe();
</script>

<template>
  <AppLayout>
    <RouterLink
      :to="{ name: 'home' }"
      class="back-link"
    >
      Back to recipes
    </RouterLink>

    <div v-if="isLoading">Loading...</div>

    <AppCard v-else-if="recipe">
      <article class="recipe">
        <div class="recipe__image-wrapper">
          <img
            class="recipe__image"
            :src="'/assets/images/' + recipe.image.url"
            :alt="recipe.image.alt"
          />
        </div>

        <h1 class="recipe__title">
          {{ recipe.title }}
        </h1>

        <p class="recipe__description">
          {{ recipe.description }}
        </p>

        <RecipeCallout title="Preparation time">
          <RecipeList
            :items="recipe.preparationTime"
            theme="secondary"
          />
        </RecipeCallout>

        <section
          v-for="(section, sectionIndex) in recipe.sections"
          :key="sectionIndex"
          class="recipe__section"
        >
          <h2 class="recipe__section-title">
            {{ section.title }}
          </h2>

          <template
            v-for="(component, componentIndex) in section.components"
            :key="componentIndex"
          >
            <RecipeList
              v-if="component.type === 'list'"
              :items="component.content"
            />

            <p v-else-if="component.type === 'paragraph'">
              {{ component.content }}
            </p>

            <RecipeTable
              v-else-if="component.type === 'table'"
              :rows="component.content"
            />
          </template>

          <hr
            v-if="sectionIndex < recipe.sections.length - 1"
            class="recipe__divider"
          />
        </section>
      </article>
    </AppCard>

    <div v-else>Recipe not found.</div>
  </AppLayout>
</template>

<style lang="scss" scoped>
  .back-link {
    margin-bottom: 2rem;
  }

  .recipe {
    display: flex;
    flex-direction: column;
    gap: 2rem;

    &__image-wrapper {
      margin: -2rem;
      margin-bottom: 0;

      @include for-tablet-portrait-up {
        margin: 0;
      }
    }

    &__image {
      aspect-ratio: 13 / 6;
      object-fit: cover;
      object-position: center;
      @include for-tablet-portrait-up {
        border-radius: 15px;
      }
    }

    &__title {
      font-family: var(--rt-font--secondary);
      font-size: 2.5rem;
      font-weight: 400;
      color: var(--rt-color-stone-900);
      line-height: 3rem;
    }

    &__section {
      display: flex;
      flex-direction: column;
      gap: 1.5rem;
    }

    &__section-title {
      font-family: var(--rt-font--secondary);
      font-size: 1.75rem;
      font-weight: 400;
      color: var(--rt-color-brown-800);
    }

    &__divider {
      width: 100%;
      height: 1px;
      background-color: var(--rt-color-stone-150);
      border: 0;
    }
  }
</style>
