<template>
  <div class="position">
    <img
      :src="position.product.thumbnail"
      :alt="position.product.title"
    >
    <div class="position__description">
      <div>
        {{ position.product.title }}
      </div>
      <div>
        quantity: {{ count }}
      </div>
      <div>
        price: {{ position.product.price }}$
      </div>
      <div>
        price for all: {{ totalPrice }}$
      </div>
    </div>
    <div class="position__action action">
      <button @click="changeQuantity(1)">
        +
      </button>
      <button @click="changeQuantity(-1)">
        -
      </button>
    </div>
  </div>
</template>

<script setup>
  import {
    computed, toRef,
  } from 'vue';

  const emit = defineEmits([
    'deletePosition',
  ]);

  const props = defineProps({
    position: {
      type: Object,
      required: true,
    },
  });

  const count = toRef(props.position, 'count');

  const totalPrice = computed(() => count.value * props.position.product.price);

  const changeQuantity = (delta) => {
    count.value += delta;
    if (count.value === 0) {
      emit('deletePosition', props.position.product.id);
    }
  };

</script>

<style lang="scss" scoped>
  .position {
    display: grid;
    grid-template-columns: 50% 1fr 1fr;
    grid-template-rows: 150px;

    &__description {
      align-self: center;

      div:first-child {
        font-weight: bold;
        font-size: 20px;
      }
    }

    &__action {
      display: grid;
      grid-template-rows: 1fr 1fr;

      button {
        justify-self: center;
        align-self: center;
        cursor: pointer;
        width: 50%;
        height: 50%;
        background-color: aliceblue;
        border-radius: 5px;
        border: 1px solid mediumpurple;
      }
    }

    img {
      height: 150px;
      max-width: 225px;
      box-shadow: -1px 3px 28px 5px rgba(34, 60, 80, 0.22);
    }
  }
</style>
