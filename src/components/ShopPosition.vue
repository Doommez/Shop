<template>
  <div
    class="shop-popup-cart__description"
  >
    <img
      :src="thumbnail"
      :alt="title"
    >
    <div>
      <div class="shop-popup-cart__description_bold">
        {{ title }}
      </div>
      <div>quantity: {{ count }}</div>
      <div>price: {{ price }}$</div>
      <div>price for all: {{ pricePositions }}$</div>
    </div>
    <div class="shop-popup-cart__description_action">
      <button @click="changeQuantity(null)">
        +
      </button>
      <button @click="changeQuantity(id)">
        -
      </button>
    </div>
  </div>
</template>

<script setup>
  import {computed, toRefs} from 'vue';

  const emit = defineEmits([
    'deletePosition',
  ]);

  const props = defineProps({
    position: {
      type: Object,
      required: true,
    },
  });

  const {
    title, count, thumbnail, price, id,
  } = toRefs(props.position);

  const pricePositions = computed(() => count.value * price.value);

  const changeQuantity = (id) => {
    if (id) {
      --count.value;
      if (count.value === 0) {
        emit('deletePosition', id);
      }
    } else {
      ++count.value;
    }
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
