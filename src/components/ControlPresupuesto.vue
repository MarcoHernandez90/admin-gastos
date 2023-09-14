<script setup>
import CircleProgress from 'vue3-circle-progress';
import "vue3-circle-progress/dist/circle-progress.css";
import { formatearCantidad } from '../helpers';
import { computed } from 'vue';

const props = defineProps({
  presupuesto: {
    type: Number,
    required: true
  },
  disponible: {
    type: Number,
    required: true
  },
  gastado: {
    type: Number,
    required: true
  }
});

const emit = defineEmits(['reset-app']);

const porcentaje = computed(() => {
  return parseInt(props.disponible * 100 / props.presupuesto);
});
</script>

<template>
  <div class="dos-columnas">
    <div class="contenedor-grafico">
      <p class="porcentaje">{{ porcentaje }}%</p>
      <CircleProgress
        :percent="porcentaje"
        :size="250"
        :border-width="25"
        :border-bg-width="25"
        :fill-color="disponible >= 0 ? '#3b82f6' : '#db2727'"
        empty-color="#e5e5e5"
      />
    </div>

    <div class="contenedor-presupuesto">
      <button class="reset-app" @click="emit('reset-app')">
        Reiniciar presupuesto
      </button>

      <p>
        <span>Presupuesto:</span>
        {{ formatearCantidad(presupuesto) }}
      </p>
      <p>
        <span>Disponible:</span>
        <span class="disponible" :class="{ rojo: disponible <= 0 }">
          {{ formatearCantidad(disponible) }}
        </span>
      </p>
      <p>
        <span>Gastado:</span>
        {{ formatearCantidad(gastado) }}
      </p>
    </div>
  </div>
</template>

<style scoped>
.dos-columnas {
  display: flex;
  flex-direction: column;
}
.dos-columnas > :first-child {
  margin-bottom: 3rem;
}
@media (min-width: 768px) {
  .dos-columnas {
    flex-direction: row;
    gap: 4rem;
    align-items: center;
  }
  .dos-columnas > :first-child {
    margin-bottom: 0;
  }
}
.reset-app {
  background-color: #db2777;
  border: none;
  padding: 1rem;
  width: 100%;
  border-radius: 1rem;
  color: var(--blanco);
  font-weight: 900;
  text-transform: uppercase;
  transition-property: background-color;
  transition-duration: 250ms;
}
.reset-app:hover {
  cursor: pointer;
  background-color: #be1a64;
}
.contenedor-presupuesto {
  width: 100%;
}
.contenedor-presupuesto p {
  font-size: 2.4rem;
  text-align: center;
  color: var(--gris-oscuro);
}
@media (min-width: 768px) {
  .contenedor-presupuesto p {
    text-align: left;
  }
}
.contenedor-presupuesto span:not(.disponible) {
  font-weight: 900;
  color: var(--azul);
}
.disponible {
  display: inline;
}
.disponible.rojo {
  color: #db2727;
}
.contenedor-grafico {
  position: relative;
}
.porcentaje {
  position: absolute;
  margin: auto;
  top: calc(50% - 1.5rem);
  left: 0;
  right: 0;
  text-align: center;
  z-index: 100;
  font-size: 3rem;
  font-weight: 900;
  color: var(--gris-oscuro);
}
</style>
