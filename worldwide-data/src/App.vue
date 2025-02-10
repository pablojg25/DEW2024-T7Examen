<script setup>
import { ref } from 'vue';

//import GoogleChart from "./components/GoogleChart.vue";

const name = ref('');

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
      <div class="div-codes"></div>
      <div class="selected"></div>
      <div class="country-data"></div>
      <div class="options"></div>
      <!-- <GoogleChart/> -->
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