<template>
  <button>
    cart <sup>{{ quantityProducts }}</sup>
  </button>
</template>
<script setup>
  import {toRef, computed} from 'vue';

  const props = defineProps({
    currentListCart: {
      type: Array,
      required: true,
    },
  });

  const listCard = toRef(props, 'currentListCart');

  const quantityProducts = computed(() => new Set(listCard.value).size);

</script>

<style lang="scss" scoped>
  button {
    font-size: 20px;
    padding-top: 0;
    border: 2px coral solid;
    border-radius: 7px;
    width: 100px;
    height: 40px;
    text-align: center;
  }
</style>
