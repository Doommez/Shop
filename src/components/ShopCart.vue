<template>
  <div class="shopping-cart">
    <button @click="$emit('update:view', !view)">
      X
    </button>
    <shop-position
      v-for="position in listPositions"
      :key="position.id"
      :position="position"
      @delete-position="deletePosition"
    />
    <div class="shopping-cart__total">
      total price: {{ totalPrice }}
    </div>
  </div>
</template>

<script setup>
  import {computed, toRef} from 'vue';
  import ShopPosition from './ShopPosition.vue';

  defineEmits([
    'update:view',
  ]);

  const props = defineProps({
    checkList: {
      type: Object,
      required: true,
    },
    view: {
      type: Boolean,
      required: true,
    },
  });

  const listPositions = toRef(props, 'checkList');

  const add = () => {
    console.log(listPositions.value);
  };

  const totalPrice = computed(() => Object.keys(listPositions.value).reduce((total, id) => total += listPositions.value[id].count * listPositions.value[id].price, 0));

  const deletePosition = (id) => delete listPositions.value[id];
</script>

<style lang="scss" scoped>
  .shopping-cart {
    display: grid;
    grid-template-rows: 30px 1fr;
    gap: 10px;

    & button {
      cursor: pointer;
      justify-self: end;
      font-size: 20px;
      font-weight: bold;
      background-color: aliceblue;
      border: none;
      outline: none;
    }

    &__total {
      justify-self: center;
      font-size: 20px;
      font-weight: bold;
    }
  }


</style>
