<script setup lang="ts">
useHead({
  title: 'Главная'
})
</script>

<template>
  <div class="home-page">
    <section class="home-page__hero hero">
      <div class="hero__container">
        <h1 class="hero__title">
          ТЕСТОВОЕ ЗАДАНИЕ
        </h1>
        <NuxtLink to="/catalog" class="hero__button">
          В каталог
        </NuxtLink>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  background: $color-primary;
  margin: 0;
  padding: 0;
  
  &__container {
    max-width: $container-width-sm;
    padding-left: $spacing-xxxl;
    width: 100%;  
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: $spacing-xl;
    @media (max-width: $breakpoint-sm) {
      padding-left: $spacing-lg;
      padding-right: 30%;
    }
  }
  
  &__title {
    font-size: $font-size-xxll;
    font-weight: 500;
    letter-spacing: $letter-spacing-md;
    margin: 0;
    text-transform: uppercase;
    color: $color-text;
    
    @media (max-width: $breakpoint-sm) {
      font-size: $font-size-title-xs;
    }
  }
  
  &__button {
    display: inline-block;
    background-color: $color-red;
    color: $color-text-light;
    text-decoration: none;
    font-size: $font-size-sm;
    font-weight: 400;
    padding: $spacing-sm $spacing-lg $spacing-sm $spacing-lg;
   
    @media (max-width: $breakpoint-sm) {
      padding: $spacing-md $spacing-xl;
      font-size: $font-size-md;
    }
  }
}
</style>