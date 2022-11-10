<template>
  <button>
    cart <sup>{{ qualityProducts }}</sup>
  </button>
</template>
<script setup>
  import {computed, toRef} from 'vue';

  const props = defineProps({
    currentListCart: {
      type: Object,
      required: true,
    },
  });
  const aaa = toRef(props, 'currentListCart');
  const qualityProducts = computed(() => {
    let counter = 0;
    for (const key in aaa.value) {
      counter += 1;
    }
    return counter;
  });
</script>

<style lang="scss" scoped>
  button {
    font-size: 20px;
    padding-top: 0px;
    border: 2px coral solid;
    border-radius: 7px;
    width: 100px;
    height: 40px;
    text-align: center;
  }
</style>
