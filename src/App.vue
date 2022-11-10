<template>
  <div class="shop">
    <shop-cart-popup
      v-if="isPopupVisible"
      :current-list-cart="currentListCart"
      :class="{'shop-popup-cart_active':isPopupVisible}"
      @change-state-visible="changeStateVisible"
    />
    <div class="shop-header">
      <shop-cart
        class="shop-header__shopping-cart"
        :current-list-cart="currentListCart"
        @click="changeStateVisible"
      />
    </div>
    <main>
      <shop-card
        v-for="product in products"
        :key="product.id"
        :product="product"
        class="chop-card"
        @add-product-in-cart="addProductInCart"
      />
    </main>
  </div>
</template>

<script setup>

  import {ref, onMounted, computed} from 'vue';
  import ShopCard from './components/ShopCard.vue';
  import ShopCartPopup from './components/ShopCartPopup.vue';
  import ShopCart from './components/ShopCart.vue';

  const products = ref([]);
  const isPopupVisible = ref(false);
  const listCart = ref([]);
  const currentListCart = computed(() => {
    const aaa = listCart.value.reduce((acc, product) => {
      if (acc[product.title]) {
        acc[product.title].quantity += 1;
        acc[product.title].priceProducts += product.price;
      } else {
        acc[product.title] = {
          price: product.price,
          quantity: 1,
          img: product.thumbnail,
          priceProducts: product.price,
        };
      }
      return acc;
    }, {});
    console.log(aaa);
    return aaa;
  });

  const getProducts = async () => (await fetch('https://dummyjson.com/products')).json();

  onMounted(async () => {
    const aaa = (await getProducts()).products;
    console.log(aaa);
    products.value = aaa;
  });
  const changeStateVisible = () => {
    isPopupVisible.value = !isPopupVisible.value;
  };
  const addProductInCart = (id) => {
    console.log('add ', id);
    const indexProduct = products.value.findIndex((product) => product.id === id);
    listCart.value.push(products.value[indexProduct]);
    console.log(listCart.value);
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
      align-items: center;
    }
  }
</style>
