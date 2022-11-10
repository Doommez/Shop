<template>
  <div class="shop">
    <shop-shopping-cart-popup
      v-if="isPopupVisible"

      :class="{'shop-popup-cart_active':isPopupVisible}"
      @changeStateVisible="changeStateVisible"
    />
    <div class="shop-header">
      <div
        class="shop-header__shopping-cart"
        @click="changeStateVisible"
      >
        Hello
      </div>
    </div>
    <main>
      <shop-card
        v-for="product in products"
        :key="product.id"
        :product="product"
        class="chop-card"
      />
    </main>
  </div>
</template>

<script setup>

  import {ref, onMounted} from 'vue';
  import ShopCard from './components/ShopCard.vue';
  import ShopShopingCart from './components/ShopShopingCart.vue';
  import ShopShoppingCartPopup from './components/ShopShoppingCartPopup.vue';

  const products = ref([]);
  const isPopupVisible = ref(true);
  const getProducts = async () => (await fetch('https://dummyjson.com/products')).json();
  // const productsReady = computed(() => {
  //   if ()
  //   products.value);
  // };

  onMounted(async () => {
    const aaa = (await getProducts()).products;
    console.log(aaa);
    products.value = aaa;
  });
  const changeStateVisible = () => {
    isPopupVisible.value = !isPopupVisible.value;
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
        border: 2px coral solid;
        border-radius: 7px;
        width: 100px;
        height: 40px;
        text-align: center;
        padding-top: 20px;
      }
    }

    main {
      display: grid;
      grid-template-columns: 15% 15% 15% 15% 15%;
      justify-content: center;
      grid-gap: 20px;
      align-items: center;
    }
  }
</style>
