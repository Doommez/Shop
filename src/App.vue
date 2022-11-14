<template>
  <div class="shop">
    <div class="shop__header">
      <button
        class="cart"
        @click="showPopup"
      >
        cart <sup>{{ Object.keys(checkList).length }}</sup>
      </button>
    </div>
    <div class="shop__list">
      <shop-product
        v-for="product in products"
        :key="product.id"
        :product="product"
        class="product"
        @add-position-to-cart="addPositionToCart"
      />
    </div>
    <div
      v-if="isShown"
      class="shop__popup"
      @click.self="showPopup"
    >
      <shop-cart
        v-model:view="isShown"
        :check-list="checkList"
      />
    </div>
  </div>
</template>

<script setup>
  import {
    ref, onMounted,
  } from 'vue';

  import ShopProduct from './components/ShopProduct.vue';
  import ShopCart from './components/ShopCart.vue';

  const products = ref([]);
  const isShown = ref(false);
  const checkList = ref({});

  const getProducts = async () => (await (await fetch('https://dummyjson.com/products')).json()).products;

  onMounted(async () => {
    products.value = await getProducts();
  });

  const addPositionToCart = (id) => {
    const indexProduct = products.value.findIndex((product) => product.id === id);
    if (checkList.value[id]) {
      checkList.value[id].count++;
    } else {
      checkList.value[id] = {
        ...products.value[indexProduct],
        count: 1,
      };
    }
  };

  const showPopup = () => isShown.value = !isShown.value;

</script>

<style lang="scss" scoped>
  .shop {
    .shop__header {
      display: grid;
      grid-template-columns: 1fr 1fr;
      padding: 20px 0;
      margin: 10px;

      .cart {
        grid-column: 2/3;
        justify-self: center;
        cursor: pointer;
        width: 20%;
        height: 40px;
        border-radius: 5px;
        border: 2px solid mediumpurple;
        background-color: aliceblue;
        font-weight: bold;
        font-size: 20px;
      }
    }

    .shop__list {
      max-width: 1360px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 1fr 1fr 1fr;
      grid-gap: 10px;
    }
  }

  .shop__popup {
    cursor: pointer;
    min-width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.8);
    position: fixed;
    top: 0;
    left: 0;
    overflow: hidden;

    & div:first-child {
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
    }
  }
</style>
