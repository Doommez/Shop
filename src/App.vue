<template>
  <div class="shop">
    <div class="shop-header">
      <div class="shop-header__shopping-cart">
        Hello
      </div>
    </div>
    <main>
      <shop-card />
    </main>
  </div>
</template>

<script setup>

  import ShopCard from './components/ShopCard.vue';
</script>

<style lang="scss" scoped>
  .shop-header {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;

    &__shopping-cart {
      grid-column: 2/-1;
      justify-self: center;
      border: 2px coral solid;
      border-radius: 7px;
      width: 100px;
      height: 50px;
      text-align: center;
      padding-top: 20px;
    }
  }
</style>
