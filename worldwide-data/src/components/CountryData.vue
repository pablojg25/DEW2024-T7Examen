<script setup>
import { computed } from 'vue';

const props = defineProps(['country']);

const PIBPerHabitant = computed(() => {
    if (props.country.pib && props.country.population) return (parseInt(props.country.pib)/parseFloat(props.country.population)).toFixed(2);
    else return 0;
});

defineEmits(['deleteCountry']);

</script>

<template>
    <div v-if="props.country">
        <p v-if="props.country.name"><strong>Nombre: </strong>{{ props.country.name }}</p>
        <p v-if="props.country.population"><strong>Población: </strong>{{ props.country.population }} millones</p>
        <p v-if="props.country.pib"><strong>PIB: </strong>{{ props.country.pib }} millones</p>
        <p v-if="props.country.area"><strong>Área: </strong>{{ props.country.area }}km<sup>2</sup></p>
        <p v-if="props.country.income"><strong>Renta: </strong>{{ props.country.income }}€</p>
        <p v-if="PIBPerHabitant"><strong>PIB/habitante: </strong><span :class="{warning: PIBPerHabitant > props.country.income}">{{ PIBPerHabitant }}</span></p>
        <button @click="$emit('deleteCountry',props.country)">Eliminar</button>
    </div>
    <p v-else>No hay datos que mostrar</p>
</template>

<style scoped>
div {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
}
div * {
    width: 50%;
}
.warning {
    color: red;
}
</style>