<script lang="ts" setup>
import type { RecipeListItem } from '@/types/Recipe';
import type { PropType } from 'vue';

defineProps({
  items: {
    type: Array as PropType<RecipeListItem[]>,
    required: true,
  },
  type: {
    type: String,
    default: 'ul',
  },
  theme: {
    type: String,
    default: 'primary',
  },
});
</script>

<template>
  <component
    :is="type"
    class="recipe-list"
    :data-type="type"
    :data-theme="theme"
  >
    <li
      v-for="(item, index) in items"
      :key="index"
      class="recipe-list__item"
    >
      <template v-if="typeof item === 'object'">
        <b>{{ item.title + ':' }}</b> {{ item.value }}
      </template>

      <template v-else>
        {{ item }}
      </template>
    </li>
  </component>
</template>

<style lang="scss" scoped>
.recipe-list {
  &[data-type='ol'] {
    list-style-type: decimal;
  }

  &[data-type='ul'] {
    --marker-font-size: 0.75rem;
    list-style-type: disc;
  }

  &[data-theme='primary'] {
    --marker-color: var(--rt-color-rose-800);
  }

  &[data-theme='secondary'] {
    --marker-color: var(--rt-color-brown-800);
  }

  &__item {
    list-style-type: inherit;
    margin-left: 1.5rem;
    padding-left: 1rem;

    &:not(:last-child) {
      margin-bottom: 1rem;
    }

    &::marker {
      font-size: var(--marker-font-size);
      color: var(--marker-color);
    }
  }
}
</style>
