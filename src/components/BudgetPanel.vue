<script setup>
import { ref, computed } from 'vue';

const budget = ref(0);

const budgetText = computed(() => {
  if (budget.value <= 0) {
    return 'Se debe insertar un presupuesto mayor que 0';
  } else {
    return `Su presupuesto es de <span>${ budget.value }€</span>`;
  }
});

defineEmits([
  'changeBudget'
]);

</script>

<template>
  <label>Indica tu presupuesto</label>
  <input type="number" v-model="budget" @change.lazy="$emit('changeBudget', budget)">
  <p v-html="budgetText"></p>
</template>

<style scoped>
body {
  background-color: lightblue;
}

p {
  font-weight: bolder;
  font-size: large;
}

span {
  color: red;
}
</style>
