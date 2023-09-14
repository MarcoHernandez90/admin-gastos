<script setup>
import { ref } from 'vue';
import LaAlerta from './LaAlerta.vue';

const presupuesto = ref(0);
const error = ref('');

const emit = defineEmits(['definir-presupuesto']);

const definirPresupuesto = () => {
  if (presupuesto.value <= 0) {
    error.value = 'Presupuesto no válido';
    setTimeout(() => {
      error.value = '';
    }, 3000);
  } else {
    error.value = '';
    emit('definir-presupuesto', presupuesto.value);
  }
}
</script>

<template>
  <form
    class="presupuesto"
    @submit.prevent="definirPresupuesto"
  >
    <LaAlerta v-if="error">
      <template v-slot:header>
        {{ error }}
      </template>
    </LaAlerta>
    <div class="campo">
      <label for="nuevo-presupuesto">Definir Presupuesto</label>
      <input
        id="nuevo-presupuesto"
        class="nuevo-presupuesto"
        placeholder="Añade tu presupuesto"
        type="number"
        min="0"
        v-model.number="presupuesto"
      >
    </div>

    <input type="submit" value="Definir Presupuesto">
  </form>
</template>

<style scoped>
.presupuesto {
  width: 100%;
}
.campo {
  display: grid;
  gap: 2rem;
}
.presupuesto input[type="number"] {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
  text-align: center;
}

.presupuesto input[type="submit"] {
  background-color: var(--azul);
  border: none;
  padding: 1rem;
  text-align: center;
  font-size: 2rem;
  margin-top: 2rem;
  color: var(--blanco);
  font-weight: 900;
  width: 100%;
  transition: background-color 250ms ease;
}
.presupuesto input[type="submit"]:hover {
  background-color: #1048a4;
  cursor: pointer;
}
.presupuesto label {
  font-size: 2.2rem;
  text-align: center;
}

</style>
