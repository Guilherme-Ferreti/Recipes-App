<script lang="ts" setup>
import type { RecipeTableRow } from '@/types/Recipe';
import type { PropType } from 'vue';

defineProps({
  rows: Array as PropType<RecipeTableRow[]>,
});
</script>

<template>
  <table class="recipe-table">
    <tr
      v-for="(row, index) in rows"
      :key="index"
    >
      <td>{{ row.title }}</td>
      <td>
        <b>{{ row.value }}</b>
      </td>
    </tr>
  </table>
</template>

<style lang="scss" scoped>
.recipe-table {
  width: 100%;
  border-spacing: 0;
  border-collapse: collapse;

  td {
    padding: 1rem 2rem;
    border-bottom: 1px solid var(--rt-color-stone-150);
  }

  tr:last-child td {
    border-bottom: none;
  }

  b {
    color: var(--rt-color-brown-800);
  }
}
</style>
