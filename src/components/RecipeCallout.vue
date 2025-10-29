<script lang="ts" setup>
defineProps({
  title: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <div class="recipe-callout">
    <h2 class="recipe-callout__title">
      {{ title }}
    </h2>
    <div class="recipe-callout__content">
      <slot />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.recipe-callout {
  padding: 1.5rem;
  background-color: var(--rt-color-rose-50);
  border-radius: 15px;
}

.recipe-callout__title {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--rt-color-rose-800);
  margin-bottom: 1rem;
}
</style>
