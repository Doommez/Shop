<template>
  <div class="product">
    <img
      :src="thumbnail"
      :alt="title"
    >
    <div>{{ title }}</div>
    <div>{{ description }}</div>
    <div>{{ price }}$</div>
    <button @click="buyProduct(id)">
      buy
    </button>
  </div>
</template>
<script setup>

  import {toRefs} from 'vue';

  const emit = defineEmits([
    'addPositionToCart',
  ]);

  const props = defineProps({
    product: {
      type: Object,
      required: true,
    },
  });

  const {
    title, description, id, thumbnail, price,
  } = toRefs(props.product);

  const buyProduct = (id) => {
    emit('addPositionToCart', id);
  };

</script>

<style lang="scss" scoped>
  .product {
    display: grid;
    grid-template-rows: 200px 30px 1fr 30px 30px;
    justify-items: center;
    border: 2px solid mediumpurple;
    border-radius: 10px;
    gap: 10px;
    padding: 10px;

    img {
      max-width: 310px;
      height: 200px;
    }

    button {
      border-radius: 5px;
      border: 1px solid mediumpurple;
      box-shadow: 1px 1px 1px mediumpurple;
      background: #ffff;
      cursor: pointer;
      width: 50%;
      transition: ease-in .1s;

      &:hover {
        transform: scale(1.1);
      }
    }
  }

</style>
