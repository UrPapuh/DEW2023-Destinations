<script setup>
// Data
const accommodationTypes = ["Albergue", "Hostal", "Bed and Breakfast", "Hotel 3*", "Hotel Superior" ];

const costsDestinations = {
  cheap: 0.7,
  moderate: 1,
  expensive: 1.3
}

const costsAccommodations = {
  "Albergue": 25,
  "Hostal": 40, 
  "Bed and Breakfast": 65,
  "Hotel 3*": 100, 
  "Hotel Superior": 200, 
}

// Program
import { ref, onMounted, computed } from 'vue';

import BudgetPanel from './components/BudgetPanel.vue';
import RangesPanel from './components/RangesPanel.vue';
import SelectPanel from './components/SelectPanel.vue';
import MapDestination from './components/MapDestination.vue';

const budget = ref(0);
const numPeople = ref(0);
const numDays = ref(0);
const accommodation = ref('');
const destinations = ref([]);
const destination = ref('');

onMounted(async () => {
  try {
    const response = await fetch('http://localhost:3000/api/destinations');
    destinations.value = await response.json();
    // destinations.value.sort((a, b) => { return a.name.toLocaleLowerCase() < b.name.toLocaleLowerCase() });
  } catch (error) {
    console.log('Error: ', error);
  }
});


const resumen = computed(() => {
  let text = '';
  if (numPeople) {
    text += `${ numPeople.value } personas `;
    if (numDays) {
      text += `durante ${ numDays.value } dias `;
      if (destination) {
        text += `en ${ destination.value } `;
        if (accommodation) {
          text += `alojandose en ${ accommodation.value }`;
        }
      }
    }
  }
  return text;
});

function changeBudget(budget) {
  budget.value = budget;
  console.log('Presupuesto: ' + budget.value);
}
function changeNumPeople(people) {
  numPeople.value = people;
  console.log('Gente: ' + numPeople.value);
}
function changeNumDays(days) {
  numDays.value = days;
  console.log('Dias: ' + numDays.value);
}
function changeAccommodation(acc) {
  accommodation.value = acc;
  console.log('Alojamiento: ' + accommodation.value);
}
function changeDestination(des) {
  destination.value = des;
  console.log('Destino: ' + destination.value);
}
</script>

<template>
  <h1>Destinos turisticos</h1>
  <p>
    {{ resumen }}
  </p>
  <BudgetPanel @changeBudget="changeBudget"/>
  <RangesPanel
    text="Numero de personas: "
    :amount="numPeople"
    :min="1"
    :max="10"
    @changeAmount="changeNumPeople"
  />
  <RangesPanel
    text="Numero de dias: "
    :amount="numDays"
    :min="1"
    :max="30"
    @changeAmount="changeNumDays"
  />
  <SelectPanel 
    text="Tipo de alojamiento"
    :options="accommodationTypes"
    @changeAccommodation="changeAccommodation"
  />
  <!-- <MapDestination 
    :lon=""
    :lat=""
    :zoom=""
  /> -->
  <p>
    <label>Selecciona un destino: </label>
    <ul>
      <li v-for="(destination, index) in destinations" :key="destination.id"
        :class="{
          expensive : destination.economic_level == 'expensive',
          moderate : destination.economic_level == 'moderate',
          cheap : destination.economic_level == 'cheap',
        }"
        @click="changeDestination(destination.name)"
      >{{ destination.name }}</li>
    </ul>
  </p>
</template>

<style scoped>
.expensive {
  color: red;
}
.moderate {
  color: orange;
}
.cheap {
  color: green;
}
</style>
