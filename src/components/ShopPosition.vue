<template>
  <div class="position">
    <img
      :src="thumbnail"
      :alt="title"
    >
    <div class="position__description">
      <div>
        {{ title }}
      </div>
      <div>
        quantity: {{ count }}
      </div>
      <div>
        price: {{ price }}$
      </div>
      <div>
        price for all: {{ pricePositions }}$
      </div>
    </div>
    <div class="position__action">
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
  .position {
    display: grid;
    grid-template-columns: 50% 1fr 1fr;
    grid-template-rows: 150px;

    & img {
      height: 150px;
      max-width: 225px;
      box-shadow: -1px 3px 28px 5px rgba(34, 60, 80, 0.22);
    }

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

      & button {
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
  }
</style>
