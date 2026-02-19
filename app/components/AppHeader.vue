<template>
  <header class="header">
    <div class="header__container">
      <div class="header__logo">
        <a href="#" class="header__logo-link">
          <div class="header__logo-square"></div>
        </a>
      </div>

      <nav class="header__nav">
        <ul class="header__nav-list">
          <li class="header__nav-item">
            <a href="#" class="header__nav-link">Мужчинам</a>
          </li>
          <li class="header__nav-item">
            <a href="#" class="header__nav-link">Женщинам</a>
          </li>
          <li class="header__nav-item">
            <a href="#" class="header__nav-link">Каталог</a>
          </li>
          <li class="header__nav-item">
            <a href="#" class="header__nav-link">Доставка</a>
          </li>
          <li class="header__nav-item">
            <a href="#" class="header__nav-link">Блог</a>
          </li>
          <li class="header__nav-item">
            <a href="#" class="header__nav-link">О нас</a>
          </li>
        </ul>
      </nav>

      <button class="header__burger" aria-label="Меню">
        <svg width="17" height="12" viewBox="0 0 17 12" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M0 11.269V9.76925H17V11.269H0ZM0 6.3845V4.8845H17V6.3845H0ZM0 1.49975V0H17V1.49975H0Z"
            fill="black" />
        </svg>
      </button>
    </div>
  </header>
</template>

<style lang="scss" scoped>
.header {
  background-color: $color-primary;
  position: sticky;
  top: 0;
  z-index: 1000;
  width: 100%;

  &__container {
    max-width: $container-width-md;
    margin: 0 auto;
    padding: 0 $spacing-xxxl;
    display: flex;
    justify-content: space-between;
    align-items: center;
    min-height: 80px;

    @media (max-width: $breakpoint-sm) {
      padding: 0 $spacing-lg;
      min-height: 70px;
    }
  }

  &__logo {
    display: flex;
    align-items: center;
    flex-shrink: 0;
  }

  &__logo-link {
    display: block;
    text-decoration: none;
  }

  &__logo-square {
    width: 42px;
    height: 42px;
    background-color: $color-red;

    @media (max-width: $breakpoint-sm) {
      width: 28px;
      height: 28px;
    }
  }

  &__nav {
    flex: 1;
    display: flex;
    justify-content: center;

    @media (max-width: $breakpoint-lg) {
      display: none;
    }
  }

  &__nav-list {
    background: $color-nav-list;
    display: flex;
    gap: $spacing-xxl;
    list-style: none;
    margin: 0;
    padding: $spacing-sm $spacing-lg $spacing-sm $spacing-lg;

    @media (max-width: $breakpoint-xl) {
      gap: $spacing-xl;
    }
  }

  &__nav-item {
    margin: 0;
    padding: 0;
  }

  &__nav-link {
    color: $color-secondary;
    text-decoration: none;
    font-size: 15px;
    font-weight: 500;
    letter-spacing: $letter-spacing-sm;
    padding: $spacing-sm 0;
    position: relative;
    white-space: nowrap;
  }

  &__burger {
    display: none;
    align-items: center; 
    justify-content: center;
    background: $color-nav-list;
    width: 40px;
    height: 40px;
    border: none;
    cursor: pointer;
    padding: 0;
    margin-left: auto;

    @media (max-width: $breakpoint-lg) {
      display: flex;  
    }   
  }

}
</style>