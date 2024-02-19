<script setup>
import { ref, watch, defineProps, onMounted } from 'vue';

const amount = ref(0);

const props = defineProps([
  'text',  
  'amount',
  'min',
  'max'
]);

const emit = defineEmits([
  'changeAmount',
]);

onMounted(() => {
  if (props.amount && props.min && props.max) {
    amount.value = props.amount >= props.min && props.amount <= props.max ? props.amount:props.min;
  }
});

watch(amount, (num) => {
  if(num < props.min) amount.value = props.min;
  if(num > props.max) amount.value = props.max;
  emit('changeAmount', amount.value);
});
</script>

<template>
  <p>
    <label>{{ props.text }}</label>
    <input type="number" v-model="amount" :min="props.min" :max="props.max">
    <input type="range" v-model="amount" :min="props.min" :max="props.max" step="1">
  </p>
</template>

<style scoped>
p {
  background-color: yellow;
}
</style>
