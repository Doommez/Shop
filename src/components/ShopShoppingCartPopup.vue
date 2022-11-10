<template>
  <div
    class="shop-popup-cart"
    @click.self="ClosePopup"
  >
    <div class="shop-popup-cart__body">
      <base-button-close
        class="shop-popup-cart__body__button"
        @click="ClosePopup"
      />
      <div>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Consectetur culpa cum cumque fugit minima nam,
        necessitatibus, nostrum porro possimus quaerat quibusdam repudiandae, vero vitae. Amet doloribus facere facilis
        perspiciatis voluptate.
      </div>
    </div>
  </div>
</template>

<script setup>
  import BaseButtonClose from './BaseButtonClose.vue';

  const emit = defineEmits([
    'changeStateVisible',
  ]);

  const ClosePopup = () => emit('changeStateVisible');

</script>

<style lang="scss" scoped>
  .shop-popup-cart {
    &_active {
      min-width: 100vw;
      height: 100vh;
      background: rgb(0, 0, 0, 0.5);
      position: fixed;
      overflow-y: hidden;
    }

    &__body {
      border-radius: 8px;
      background-color: aliceblue;
      padding: 20px;
      top: 40%;
      left: 40%;
      position: fixed;
      max-width: 20%;
      display: grid;
      grid-template-rows: 20px 1fr;
      justify-items: end;
      z-index: 10;

      &__button {
        width: 10px;
        height: 10px;
        grid-row: 1/2;
      }
    }
  }
</style>
