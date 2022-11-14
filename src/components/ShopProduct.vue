<template>
  <div class="product">
    <img
      :src="product.thumbnail"
      :alt="product.title"
    >
    <div>
      {{ props.product.title }}
    </div>
    <div>
      {{ product.description }}
    </div>
    <div>
      {{ product.price }}$
    </div>
    <button @click="buyProduct(product.id)">
      buy
    </button>
  </div>
</template>
<script setup>

  const emit = defineEmits([
    'addPositionToCart',
  ]);

  const props = defineProps({
    product: {
      type: Object,
      required: true,
    },
  });

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
