<template>
  <div class="shop">
    <!--    <shop-cart-popup-->
    <!--      v-if="show"-->
    <!--      v-model:show="show"-->
    <!--      :current-list-cart="listCart"-->
    <!--      :class="{'shop-popup-cart_active':show}"-->
    <!--    />-->

    <div
      v-if="show"
      class="shop-popup-cart"
      :current-list-cart="listCart"
      :class="{'shop-popup-cart_active':show}"
      @click.self="show = !show"
    >
      <div class="shop-popup-cart__body">
        <button
          @click="show = !show"
        >
          close
        </button>
        <shop-position
          v-for="position in checkList"
          :key="position.id"
          :position="position"
          @delete-position="deletePosition"
        />
        <div class="shop-popup-cart__total">
          total price: {{ totalPrice }}
        </div>
      </div>
    </div>

    <div class="shop-header">
      <button
        @click="show = !show"
      >
        cart <sup>{{ Object.keys(checkList).length }}</sup>
      </button>
    </div>
    <main>
      <shop-card
        v-for="product in products"
        :key="product.id"
        :product="product"
        class="chop-card"
        @add-product-in-cart="addProductToCart"
      />
    </main>
  </div>
</template>

<script setup>

  import {
    ref, onMounted, watch, computed,
  } from 'vue';
  import ShopCard from './components/ShopCard.vue';
  import ShopCartPopup from './components/ShopCartPopup.vue';
  import ShopPosition from './components/ShopPosition.vue';



  const products = ref([]);
  const show = ref(false);
  const checkList = ref({});
  const listCart = ref([]);

  const getProducts = async () => {
    const response = await (await fetch('https://dummyjson.com/products')).json();
    return response.products;
  };

  onMounted(async () => {
    products.value = await getProducts();
  });

  const totalPrice = computed(() => Object.keys(checkList.value).reduce((total, id) => total += checkList.value[id].count * checkList.value[id].price, 0));

  const addProductToCart = (id) => {
    const indexProduct = products.value.findIndex((product) => product.id === id);
    if (checkList.value[id]) {
      checkList.value[id].count++;
    } else {
      checkList.value[id] = products.value[indexProduct];
      checkList.value[id].count = 1;
    }
    console.log(checkList.value);
    // if (checkList.value[id]) {
    //   console.log(checkList.value[id], 'yes');
    //   checkList.value[id] = {
    //     ...products.value[indexProduct],
    //     quantity: products.value[indexProduct].quantity + 1,
    //     priceProducts: products.value[indexProduct].priceProducts + products.value[indexProduct].price,
    //   };
    // } else {
    //   console.log(checkList.value[id], 'is not');
    //   checkList.value[id] = {
    //     ...products.value[indexProduct],
    //     quantity: 1,
    //     priceProducts: products.value[indexProduct].price,
    //   };
    // }
    // console.log(checkList.value);
  };

  const deletePosition = (id) => {
    console.log(id, checkList.value);
    delete checkList.value[id];
    console.log(checkList.value);
  };
</script>

<style lang="scss" scoped>
  .shop {
    min-height: 100vh;

    .shop-header {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      padding: 20px 0;
      margin: 10px;

      &__shopping-cart {
        grid-column: 2/-1;
        justify-self: center;
        cursor: pointer;
      }
    }

    main {
      display: grid;
      grid-template-columns: 15% 15% 15% 15% 15%;
      justify-content: center;
      grid-gap: 20px;
    }
  }
</style>
