<template>
  <div class="shop__popup">
    <div class="shopping-cart">
      <button @click="$emit('update:view', false)">
        X
      </button>
      <shop-cart-position
        v-for="position in checkList"
        :key="position.product.id"
        :position="position"
        @delete-position="$emit('deletePosition', position.product.id)"
      />
      <div class="shopping-cart__total">
        total price: {{ totalPriceForAll }}
      </div>
    </div>
  </div>
</template>

<script setup>
  import {computed} from 'vue';
  import ShopCartPosition from './ShopCartPosition.vue';

  defineEmits([
    'update:view',
    'deletePosition',
  ]);

  const props = defineProps({
    checkList: {
      type: Object,
      required: true,
    },
    view: {
      type: Boolean,
      required: true,
    },
  });

  const totalPriceForAll = computed(() => Object.keys(props.checkList).reduce((total, id) => total += props.checkList[id].count * props.checkList[id].product.price, 0));


</script>

<style lang="scss" scoped>
  .shop__popup {
    cursor: pointer;
    min-width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.8);
    position: fixed;
    top: 0;
    left: 0;
    overflow: hidden;
  }

  .shopping-cart {
    cursor: auto;
    border-radius: 8px;
    background-color: aliceblue;
    padding: 20px;
    top: 20vh;
    left: calc(50% - 20%);
    position: absolute;
    width: 40%;
    height: 50%;
    overflow: scroll;
    display: grid;
    grid-template-rows: 30px 1fr;
    gap: 10px;

    &__total {
      justify-self: center;
      font-size: 20px;
      font-weight: bold;
    }

    button {
      cursor: pointer;
      justify-self: end;
      font-size: 20px;
      font-weight: bold;
      background-color: aliceblue;
      border: none;
      outline: none;
    }
  }
</style>
