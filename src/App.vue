<template>
  <div class="shop">
    <div class="shop-header">
      <button
        class="shop-header__cart"
        @click="show = !show"
      >
        cart <sup>{{ Object.keys(checkList).length }}</sup>
      </button>
    </div>
    <div class="shop-list">
      <shop-product
        v-for="product in products"
        :key="product.id"
        :product="product"
        class="product"
        @add-position-to-cart="addPositionToCart"
      />
    </div>
  </div>
  <div
    v-if="show"
    class="shop-popup"
    @click.self="show = !show"
  >
    <div class="shop-popup__body">
      <button
        @click="show = !show"
      >
        X
      </button>
      <shop-position
        v-for="position in checkList"
        :key="position.id"
        :position="position"
        @delete-position="deletePosition"
      />
      <div class="shop-popup__total">
        total price: {{ totalPrice }}
      </div>
    </div>
  </div>
</template>

<script setup>
  import {
    ref, onMounted, computed,
  } from 'vue';
  import ShopPosition from './components/ShopPosition.vue';
  import ShopProduct from './components/ShopProduct.vue';




  const products = ref([]);
  const show = ref(false);
  const checkList = ref({});

  const getProducts = async () => (await (await fetch('https://dummyjson.com/products')).json()).products;

  onMounted(async () => {
    products.value = await getProducts();
  });

  const totalPrice = computed(() => Object.keys(checkList.value).reduce((total, id) => total += checkList.value[id].count * checkList.value[id].price, 0));

  const addPositionToCart = (id) => {
    const indexProduct = products.value.findIndex((product) => product.id === id);
    if (checkList.value[id]) {
      checkList.value[id].count++;
    } else {
      checkList.value[id] = products.value[indexProduct];
      checkList.value[id].count = 1;
    }
  };

  const deletePosition = (id) => delete checkList.value[id];
</script>

<style lang="scss" scoped>
  .shop {
    min-height: 100vh;

    .shop-header {
      display: grid;
      grid-template-columns: 1fr 1fr;
      padding: 20px 0;
      margin: 10px;

      &__cart {
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

    .shop-list {
      max-width: 1360px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 1fr 1fr 1fr;
      grid-gap: 10px;
    }
  }
  .shop-popup{
    cursor: pointer;
    min-width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.8);
    position: fixed;
    top: 0;
    left: 0;
    overflow: hidden;
    &__body{
      cursor: auto;
      border-radius: 8px;
      background-color: aliceblue;
      padding: 20px;
      top: 20vh;
      left: calc(50% - 20%);
      position: absolute;
      width: 40%;
      height: 50%;
      display: grid;
      grid-template-rows: 30px 1fr;
      gap: 10px;
      overflow: scroll;
      & button{
        cursor: pointer;
        justify-self: end;
        font-size: 20px;
        font-weight: bold;
        background-color: aliceblue;
        border: none;
        outline: none;
      }
    }
    &__total{
      justify-self: center;
    }
  }
</style>
