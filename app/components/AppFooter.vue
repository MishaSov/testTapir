<template>
  <footer class="footer">
    <div class="footer__container">
      <div class="footer__column">
        <h3 class="footer__column-title">Каталог</h3>
        <ul class="footer__menu">
          <li><a href="#">Все бренды</a></li>
          <li><a href="#">Nike</a></li>
          <li><a href="#">Adidas</a></li>
          <li><a href="#">Puma</a></li>
          <li><a href="#">New Balance</a></li>
        </ul>
      </div>

      <div class="footer__column">
        <h3 class="footer__column-title">Помощь</h3>
        <ul class="footer__menu">
          <li><a href="#">Доставка</a></li>
          <li><a href="#">Заказ и оплата</a></li>
          <li><a href="#">Обмен и Возврат</a></li>
          <li><a href="#">F. A. Q.</a></li>
        </ul>
      </div>

      <div class="footer__column">
        <h3 class="footer__column-title">Информация</h3>
        <ul class="footer__menu">
          <li><a href="#">Преимущества</a></li>
          <li><a href="#">Гарантия и безопасность</a></li>
          <li><a href="#">Индивидуальный заказ</a></li>
          <li><a href="#">Акции</a></li>
        </ul>
      </div>

      <div class="footer__column">
        <h3 class="footer__column-title">Компания</h3>
        <ul class="footer__menu">
          <li><a href="#">О нас</a></li>
          <li><a href="#">Блог</a></li>
          <li><a href="#">Отзывы</a></li>
          <li><a href="#">Контакты</a></li>
        </ul>
      </div>
    </div>
  </footer>
</template>

<style lang="scss" scoped>
.footer {
  background-color: $color-secondary;
  
  &__container {
    max-width: $container-width-sm;
    margin: 0 auto;
    padding: $spacing-xxxxl $spacing-xxxl;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: $spacing-lg;
   
    @media (max-width: $breakpoint-sm) {
      grid-template-columns: 1fr;
      padding: $spacing-xxxl $spacing-xxxl $spacing-xxxl $spacing-lg;
    }
  }
  
  &__column-title {
    color: $color-text-footer;
    font-size: $font-size-sm;
    font-weight: 400;
    margin: 0 0 $spacing-sm 0;
  }
  
  &__menu {
    list-style: none;
    margin: 0;
    padding: 0;
    
    li {
      margin-bottom: $spacing-sm;
    }
    
    a {
      color: $color-primary;
      text-decoration: none;
      font-size: $font-size-sm;
    }
  }
}
</style>