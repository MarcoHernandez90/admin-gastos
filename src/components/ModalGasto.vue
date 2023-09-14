<script setup>
import { ref } from 'vue'
import LaAlerta from './LaAlerta.vue'
import cerrarModal from '../assets/img/cerrar.svg'

const error = ref('')

const emit = defineEmits([
  'agregar-gasto',
  'ocultar-modal',
  'update:nombre',
  'update:categoria',
  'update:cantidad',
  'eliminar-gasto'
])
const props = defineProps({
  modal: {
    type: Object,
    required: true
  },
  nombre: {
    type: String,
    required: true
  },
  cantidad: {
    type: [String, Number],
    required: true
  },
  categoria: {
    type: String,
    required: true
  },
  id: {
    type: String
  }
})

const agregarGasto = () => {
  const { cantidad, nombre, categoria } = props
  if ([cantidad, nombre, categoria].includes('')) {
    error.value = 'Todos los campos son obligatorios'
    setTimeout(() => {
      error.value = ''
    }, 3000)
    return
  }

  if (cantidad <= 0) {
    error.value = 'Cantidad no válida'
    setTimeout(() => {
      error.value = ''
    }, 3000)
    return
  }

  emit('agregar-gasto')
}
</script>

<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img :src="cerrarModal" alt="x" @click="emit('ocultar-modal')" />
    </div>
    <div
      class="contenedor contenedor-formulario"
      :class="modal.animar ? 'animar' : 'cerrar'"
    >
      <form class="nuevo-gasto" @submit.prevent="agregarGasto">
        <legend>{{ id ? 'Editar Gasto' : 'Agregar Gasto' }}</legend>
        <LaAlerta v-if="error">
          <template v-slot:header>{{ error }}</template>
        </LaAlerta>
        <div class="campo">
          <label for="nombre">Nombre del Gasto:</label>
          <input
            type="text"
            name="nombre"
            id="nombre"
            placeholder="Escribe un nombre para el gasto"
            :value="nombre"
            @input="emit('update:nombre', $event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="cantidad">Cantidad:</label>
          <input
            type="number"
            name="cantidad"
            id="cantidad"
            placeholder="Escribe la cantidad del gasto, ej. 600"
            :value="cantidad"
            @input="emit('update:cantidad', +$event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="categoria">Categoría:</label>
          <select
            name="categoria"
            id="categoria"
            :value="categoria"
            @input="emit('update:categoria', $event.target.value)"
          >
            <option value="">-- Seleccione --</option>
            <option value="ahorro">Ahorro</option>
            <option value="comida">Comida</option>
            <option value="casa">Casa</option>
            <option value="varios">Gastos Varios</option>
            <option value="ocio">Ocio</option>
            <option value="salud">Salud</option>
            <option value="suscripciones">Suscripciones</option>
          </select>
        </div>
        <input type="submit" :value="id ? 'Guardar Gasto' : 'Añadir Gasto'" />
      </form>

      <button
        v-if="id"
        class="btn-eliminar"
        type="button"
        @click="emit('eliminar-gasto', id)"
      >
        Eliminar Gasto
      </button>
    </div>
  </div>
</template>

<style scoped>
.modal {
  position: absolute;
  background-color: rgb(0 0 0 / 0.9);
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
}

.cerrar-modal {
  position: absolute;
  right: 3rem;
  top: 3rem;
}

.cerrar-modal img {
  width: 3rem;
  cursor: pointer;
}

.contenedor-formulario {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}

.contenedor-formulario.animar {
  opacity: 1;
}

.contenedor-formulario.cerrar {
  opacity: 0;
}

.nuevo-gasto {
  margin: 10rem auto 0 auto;
  display: grid;
  gap: 2rem;
}

.nuevo-gasto legend {
  text-align: center;
  color: var(--blanco);
  font-size: 3rem;
  font-weight: 700;
}

.nuevo-gasto input,
.nuevo-gasto select {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
}

.nuevo-gasto input[type='submit'] {
  background-color: var(--azul);
  color: var(--blanco);
  font-weight: 700;
  cursor: pointer;
}

.nuevo-gasto label {
  color: var(--blanco);
  font-size: 3rem;
}

.campo {
  display: grid;
  gap: 2rem;
}
.btn-eliminar {
  border: none;
  border-radius: 1rem;
  padding: 1rem;
  width: 100%;
  background-color: #ef4444;
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--blanco);
  margin-top: 10rem;
  cursor: pointer;
}
</style>
