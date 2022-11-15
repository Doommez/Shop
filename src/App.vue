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
        @add-position-to-cart="addPositionToCart"
      />
    </div>
    <shop-cart
      v-if="isShown"
      v-model:view="isShown"
      :check-list="checkList"
      class="shop__popup"
      @delete-position="deletePosition"
    />
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
        product: products.value[indexProduct],
        count: 1,
      };
    }
  };

  const showPopup = () => isShown.value = true;

  const deletePosition = (id) => delete checkList.value[id];

</script>

<style lang="scss" scoped>
  .shop {
    &__header {
      display: grid;
      grid-template-columns: 1fr 1fr;
      padding: 20px 0;
      margin: 10px;
    }

    &__list {
      max-width: 1360px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 1fr 1fr 1fr;
      grid-gap: 10px;
    }
  }

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

</style>
