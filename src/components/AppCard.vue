<template>
  <div class="app-card">
    <slot />
  </div>
</template>

<style lang="scss" scoped>
.app-card {
  background-color: var(--rt-color-white);
  max-width: 44rem;
  padding: 2rem;
  width: 100%;

  @include for-tablet-portrait-up {
    border-radius: 15px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  }
}
</style>
