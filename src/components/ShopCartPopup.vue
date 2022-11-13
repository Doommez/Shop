<template>
  <div
    class="shop-popup-cart"
    :value="show"
    @click.self="$emit('update:show', !show)"
  >
    <div class="shop-popup-cart__body">
      <button
        :value="show"
        @click="$emit('update:show', !show)"
      >
        close
      </button>
      <div
        v-for="product in currentProducts"
        :key="product.id"
        class="shop-popup-cart__description"
      >
        <img
          :src="product.thumbnail"
          :alt="product.title"
        >
        <div>
          <div class="shop-popup-cart__description_bold">
            {{ product.title }}
          </div>
          <div>quantity: {{ product.quantity }}</div>
          <div>price: {{ product.price }}$</div>
          <div>price for all: {{ product.priceProducts }}$</div>
        </div>
        <div class="shop-popup-cart__description_action">
          <button @click="addProduct(product.id)">
            +
          </button>
          <button @click="deleteProduct(product.id)">
            -
          </button>
        </div>
      </div>
      <div class="shop-popup-cart__total">
        total price: {{ totalPrice }}
      </div>
    </div>
  </div>
</template>

<script setup>
  import {toRef, computed} from 'vue';


  const emit = defineEmits([
    'update:show',
  ]);

  const props = defineProps({
    currentListCart: {
      type: Array,
      required: true,
    },
    show: {
      type: Boolean,
      required: true,
    },
  });

  const listCart = toRef(props, 'currentListCart');

  const totalPrice = computed(() => listCart.value.reduce((total, product) => total += product.priceProducts, 0));

  const currentProducts = computed(() => listCart.value.reduce((listProduct, product) => {
    if (listProduct.indexOf(product) !== -1) {
      product.quantity += 1;
      product.priceProducts += product.price;
      listProduct[listProduct.indexOf(product)] = product;
    } else {
      product.quantity = 1;
      product.priceProducts = product.price;
      listProduct.push(product);
    }
    return listProduct;
  }, []));

  const addProduct = (id) => {
    const indexProduct = listCart.value.findIndex((product) => product.id === id);
    if (indexProduct !== -1) {
      listCart.value.push(listCart.value[indexProduct]);
    }
  };

  const deleteProduct = (id) => {
    const indexProduct = listCart.value.findIndex((product) => product.id === id);
    if (indexProduct !== -1) {
      listCart.value.splice(indexProduct, 1);
    }
  };

  const ClosePopup = () => {
    emit('changeStateVisible');
  };

</script>

<style lang="scss" scoped>
  .shop-popup-cart {
    cursor: pointer;

    &_active {
      min-width: 100vw;
      height: 100vh;
      background: rgba(0, 0, 0, 0.8);
      position: fixed;
      overflow: hidden;
    }

    &__button-close {
      width: 10px;
      height: 10px;
      justify-self: end;
    }

    &__body {
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
      grid-template-rows: 20px repeat(auto-fill, 90px);
      gap: 10px;
      overflow: scroll;

      &::-webkit-scrollbar {
        width: 0;
        height: 0;
      }

      img {
        max-width: 85px;
        max-height: 85px;
        box-shadow: -1px 3px 28px 5px rgba(34, 60, 80, 0.22);
      }

    }
    &__total {
      font-size: 20px;
      font-weight: bold;
      justify-self: center;
    }
    &__description {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      justify-items: center;

      &_bold {
        font-size: 18px;
        font-weight: bold;
      }

      &_action {
        display: grid;
        grid-template-rows: 50% 50%;

        button {
          width: 80px;
          height: 30px;
          background-color: aliceblue;
          border: 1px solid mediumpurple;
          border-radius: 5px;
          cursor: pointer;
        }
      }
    }
  }
</style>
