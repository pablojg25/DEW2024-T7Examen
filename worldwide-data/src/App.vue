<script setup>
import { onMounted, ref } from 'vue';
import codes from './data/codes.json';
import GoogleChart from "./components/GoogleChart.vue";
import CountryData from './components/CountryData.vue';

const name = ref('');
const selectedCountries = ref([]);
const countryNames = ref([]);
const currentCountry = ref();
const dataTypes = ['population', 'pib', 'area', 'income'];
const option = ref('');

onMounted(async () => {
    const response = await fetch('http://localhost:3000/api/names');
    countryNames.value = await response.json();
})

async function selectCountry (country) {
  try {
    const response = await fetch('http://localhost:3000/api/country/' + country);
    if (response.ok) {
      const data = await response.json();
      if (!selectedCountries.value.includes(countryNames.value[country])) {
        selectedCountries.value.push(countryNames.value[country]);
        selectedCountries.value.sort((a,b) => a.localeCompare(b));
      }
      currentCountry.value = data;
    } else {
      throw new Error("País no encontrado");
    }
  } catch (ex) {
    console.log(ex);
  }
}

/*
* Elimina país de la lista de seleccionados
**/
function removeCountry (country) {
  const index = selectedCountries.value.findIndex((code) => code == country);
  selectedCountries.value.splice(index,1);
}

/*
* Elimina país de la API
**/
async function deleteCountry (country) {
  const response = await fetch('http://localhost:3000/api/country/'+country.code,{method: 'DELETE'});
  currentCountry.value = null;
  countryNames.value[country.code] = null;
  removeCountry(country.name);
}

function changeOption (newOption) {
  option.value = newOption;
}

</script>

<template>
    <div class="parent">
      <div class="header">
        <h1>Datos mundiales</h1>
      </div>
      <div class="name">
        <p>
            <label for="">Inserta tu nombre: </label>
            <input type="text" v-model="name">
        </p>
        <p v-if="name">Tu nombre <strong>{{ name }}</strong> tiene <strong>{{ name.length }}</strong> letras</p>
      </div>
      <div class="div-codes">
        <h2>Códigos: </h2>
        <ul>
            <li v-for="code in codes" @click="selectCountry( code )">{{ countryNames[code] }}</li>
        </ul>
      </div>
      <div class="selected">
        <h2 v-if="selectedCountries.length == 0">Debes seleccionar algún país en el panel de códigos</h2>
        <div v-else>
            <h2>Seleccionados ({{ selectedCountries.length }}): </h2>
            <ul>
                <li v-for="country in selectedCountries">{{ country }} <button @click="removeCountry(country)">Desmarcar</button></li>
            </ul>
        </div>
      </div>
      <div class="country-data">
        <CountryData v-if="currentCountry" :country="currentCountry" @deleteCountry="deleteCountry"/>
      </div>
      <div class="options">
        <p v-for="type in dataTypes" style="display:inline;">
          <input type="radio" name="option" :id=type @click="changeOption(type)">
          <label :for=type>{{type}}</label>
        </p>
      </div>
      <div class="chart"><GoogleChart/></div>
    </div>
  </template>
  
  <style scoped>
  .parent {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: auto repeat(3, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 0px;
    height: 100vh;
    margin: -2rem;
  }
  
  .header {
    grid-area: 1 / 1 / 2 / 6;
    background-color: aquamarine;
  }
  
  .header h1 {
    margin: 2px;
  }
  
  .div-codes {
    grid-area: 2 / 1 / 6 / 2;
    background-color: azure;
    overflow-y: auto;
  }
  
  .name {
    grid-area: 2 / 2 / 3 / 3;
    background-color: antiquewhite;
  }
  
  .selected {
    grid-area: 2 / 5 / 4 / 6;
    background-color: lightyellow;
    overflow-y: auto;
  }
  
  .country-data {
    grid-area: 2 / 3 / 3 / 5;
    background-color: lightseagreen;
  }
  
  .options {
    grid-area: 3 / 2 / 4 / 5;
    background-color: bisque;
  }
  
  .chart {
    grid-area: 4 / 2 / 5 / 6;
    background-color: aqua;
  }
  
  ul li {
    text-align: left;
    font-size: 0.9rem;
  }
  
  ul li:hover {
    font-weight: bold;
    cursor: pointer
  }
  
  ul li button {
    font-size: 0.6rem;
    margin: 2px;
  }
  
  h2 {
    font-size: 1.2rem;
  }
  </style>