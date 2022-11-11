<template>
  <div
    class="shop-popup-cart"
    @click.self="ClosePopup"
  >
    <div class="shop-popup-cart__body">
      <base-button-close
        class="shop-popup-cart__button-close"
        @click="ClosePopup"
      />
      <div
        v-for="card in currentProducts"
        :key="card.id"
        class="shop-popup-cart__description"
      >
        <img
          :src="card.thumbnail"
          :alt="card.title"
        >
        <div>
          <div class="shop-popup-cart__description_bold">
            {{ card.title }}
          </div>
          <div>quantity: {{ card.quantity }}</div>
          <div>price: {{ card.price }}$</div>
          <div>price for all: {{ card.priceProducts }}$</div>
        </div>
        <div class="shop-popup-cart__description_action">
          <button @click="addProduct(card.id)">
            +
          </button>
          <button @click="deleteProduct(card.id)">
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
  import BaseButtonClose from './BaseButtonClose.vue';

  const emit = defineEmits([
    'changeStateVisible',
  ]);

  const props = defineProps({
    currentListCart: {
      type: Object,
      required: true,
    },
  });

  const listCart = toRef(props, 'currentListCart');

  const totalPrice = computed(() => listCart.value.reduce((acc, product) => acc += product.priceProducts, 0));

  const currentProducts = computed(() => {
    const aaa = listCart.value.reduce((acc, product) => {
      console.log(acc.indexOf(product));
      if (acc.indexOf(product) !== -1) {
        product.quantity += 1;
        product.priceProducts += product.price;
        acc[acc.indexOf(product)] = product;
      } else {
        product.quantity = 1;
        product.priceProducts = product.price;
        acc.push(product);
      }
      return acc;
    }, []);
    console.log(aaa);
    return aaa;
  });

  const addProduct = (id) => {
    const indexProduct = listCart.value.findIndex((product) => product.id === id);
    console.log(indexProduct, listCart.value);
    if (indexProduct !== -1) {
      listCart.value.push(listCart.value[indexProduct]);
      // listCart.value[indexProduct].priceProducts += listCart.value[indexProduct].price;
      // listCart.value[indexProduct].quantity += 1;
    }
  };
  // const arrCardKey = Object.keys(listCart.value);
  // arrCardKey.forEach((key) => {
  //   if (listCart.value[`${key}`].id === id) {
  //     console.log(listCart.value[`${key}`]);
  //     console.log(listCart.value);
  // listCart.value = {
  //   ...listCart.value,
  //   [`${key}`]: {
  //     title: listCart.value[`${key}`].title,
  //     price: listCart.value[`${key}`].price,
  //     img: listCart.value[`${key}`].thumbnail,
  //     id: listCart.value[`${key}`].id,
  //     quantity: listCart.value[`${key}`].quantity + 1,
  //     priceProducts: listCart.value[`${key}`].priceProducts + listCart.value[`${key}`].price,
  //   },
  //
  // };
  // }
  // });

  //
  //   console.log(arrCardKey);
  //   // for (const key in listCart.value) {
  //   //   if (listCart.value[`${key}`].id === id) {
  //   //     listCart.value[`${key}`].quantity += 1;
  //   //     listCart.value[`${key}`].priceProducts += listCart.value[`${key}`].price;
  //   //   }
  //   // }
  // };
  const deleteProduct = (id) => {
    console.log(id);
    const indexProduct = listCart.value.findIndex((product) => product.id === id);
    if (indexProduct !== -1) {
      listCart.value.splice(indexProduct, 1);
    }
  };

  const ClosePopup = () => {
    document.body.style = '';
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
