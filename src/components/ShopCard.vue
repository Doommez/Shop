<template>
  <div>
    <img
      :src="thumbnail"
      :alt="title"
    >
    <div>{{ title }}</div>
    <div>{{ description }}</div>
    <div>{{ price }}$</div>
    <button @click="upIdProduct(id)">
      buy
    </button>
  </div>
</template>
<script setup>

  import {toRefs} from 'vue';

  const emit = defineEmits([
    'addProductInCart',
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

  const upIdProduct = (id) => {
    emit('addProductInCart', id);
  };

</script>

<style lang="scss" scoped>
  .chop-card {
    display: grid;
    grid-template-rows: 1fr 30px 100px 40px 30px;
    justify-items: center;
    border: 1px solid mediumpurple;
    border-radius: 10px;
    padding: 0 0 20px 0;

    div {
      padding: 20px 10px;
    }

    img {
      padding: 20px 10px;
      max-width: 200px;
      height: 150px;
    }

    button {
      border-radius: 5px;
      border: 1px solid mediumpurple;
      box-shadow: 1px 1px 1px mediumpurple;
      background: #ffff;
      cursor: pointer;
      width: 50%;
      transition: ease-in .1s;
    }

    button:hover {
      transform: scale(1.2);
    }
  }

</style>
