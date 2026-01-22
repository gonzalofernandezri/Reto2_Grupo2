<template>
    <div class="w-full max-w-6xl mx-auto p-6 flex flex-col items-center gap-6">
      <div class="flex flex-col sm:flex-row gap-4 w-full">
        <select v-model="tipo" @change="cambiarFiltros"  class="border p-2 rounded flex-1">
          <option value="">Todos</option>
          <option value="Presentación">Presentación</option>
          <option value="Charla">Charla</option>
          <option value="Taller">Taller</option>
          <option value="Mesa Redonda">Mesa Redonda</option>
          <option value="Exhibición">Exhibición</option>
          <option value="Torneo">Torneo</option>
          <option value="Networking">Networking</option>
          <option value="Competición">Competición</option>
        </select>
      
        <input 
          type="date" 
          class="border p-2 rounded flex-1"
          min="2025-02-13" 
          max="2025-02-15"
          v-model="fecha" 
          @change="cambiarFiltros">

        <label class="flex items-center gap-2 flex-1">
          <input type="checkbox" v-model="soloConPlazas" @change="cambiarFiltros">
          Plazas libres
        </label>
      </div>

      <ul class="w-full grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
        <li v-for="evento in eventos" :key="evento.id" class="border rounded-lg shadow-md overflow-hidden flex flex-col">
          
          <!-- Imagen (si tienes URL) -->
          <img 
            v-if="evento.imagen" 
            :src="`/gamefest_resources/events/${evento.imagen}`" 
            alt="Imagen del evento" 
            class="w-full h-48 object-cover"
          />
          <div class="p-4 flex flex-col gap-2">
            <strong class="text-xl">{{ evento.titulo }}</strong>
            <div class="text-gray-600 text-sm">
              Tipo: {{ evento.tipo }}<br>
              Fecha: {{ evento.fecha }}<br>
              Hora: {{ evento.hora }}<br>
              Plazas libres: {{ evento.plazasLibres }}
            </div>
            <p class="mt-2 text-gray-700 text-sm">
              {{ evento.descripcion }}
            </p>
          </div>
          
        </li>
      </ul>

      <div class="flex gap-4">
        <button 
          @click="anteriorPagina" 
          :disabled="paginaActual === 1"
          class="bg-blue-500 text-white px-4 py-2 rounded disabled:bg-gray-300 disabled:text-gray-600">
          Anterior
        </button>
        
        <button 
          @click="siguientePagina" 
          :disabled="paginaActual === totalPaginas"
          class="bg-blue-500 text-white px-4 py-2 rounded disabled:bg-gray-300 disabled:text-gray-600">
          Siguiente
        </button>
      </div>
    </div>
</template>


<script setup>
import { ref, onMounted} from 'vue'

const eventos = ref([])
const tipo = ref('')
const fecha = ref('')
const soloConPlazas = ref(false)

var paginaActual = 1
var totalPaginas = 2
const eventosPorPagina = 9

async function calcularPaginas(){
  const params = new URLSearchParams()
  
  if (tipo.value) {
    params.append('tipo', tipo.value)
  }

  if (fecha.value) {
    params.append('fecha', fecha.value)
  }

  if (soloConPlazas.value){
    params.append('plazas', 'true')
  } 

  const res = await fetch(`/api/eventostodo_api.php?${params.toString()}`)
  const data = await res.json();
  totalPaginas = Math.ceil(Number(data) / eventosPorPagina);
  console.log(totalPaginas)
}

async function cargarEventos() {
  const params = new URLSearchParams()

  if (tipo.value) {
    params.append('tipo', tipo.value)
  }

  if (fecha.value) {
    params.append('fecha', fecha.value)
  }

  if (soloConPlazas.value){
    params.append('plazas', 'true')
  } 

  params.append('pagina', paginaActual)

  const res = await fetch(`/api/eventos_api.php?${params.toString()}`)
  const data = await res.json()
  console.log(data)
  eventos.value = data;
}

function cambiarFiltros() {
  paginaActual = 1
  calcularPaginas()
  cargarEventos()
}

function siguientePagina() {
  if (paginaActual < totalPaginas) {
    paginaActual++
    cargarEventos()
  }
}

function anteriorPagina() {
  if (paginaActual > 1) {
    paginaActual--
    cargarEventos()
  }
}
onMounted(() => {
  cargarEventos();
  calcularPaginas();
});
</script>

