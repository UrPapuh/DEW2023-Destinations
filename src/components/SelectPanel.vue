<script setup>
import { ref, watch, defineProps} from 'vue';

const accommodation = ref('');

const props = defineProps([
  'text',  
  'options'
]);

const emit = defineEmits([
  'changeAccommodation',
]);

watch(accommodation, (acc) => {
  if (acc) emit('changeAccommodation', acc);
});

</script>

<template>
  <p>
    {{ props.text }}
    <select v-model="accommodation">
      <option value="">- Elige un tipo -</option>
      <option v-for="(option, index) in props.options" :key="index">{{ option }}</option>
    </select>
  </p>
</template>

<style scoped>

</style>
