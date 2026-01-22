<template>
    <div>
        <select v-model="tipo" @change="cambiarFiltros">
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
          class="border p-2 rounded w-full" 
          min="2025-02-13" 
          max="2025-02-15"
          v-model="fecha" 
          @change="cambiarFiltros">

        <label class="flex items-center gap-2 mt-2">
          <input type="checkbox" v-model="soloConPlazas" @change="cambiarFiltros">
          Solo eventos con plazas disponibles
        </label>

        <ul>
            <li v-for="evento in eventos" :key="evento.id">
              <strong>{{ evento.titulo }}</strong><br>
              Tipo: {{ evento.tipo }}<br>
              Fecha: {{ evento.fecha }}<br>
              Hora: {{ evento.hora }}<br>
              Plazas libres: {{ evento.plazasLibres }}<br>
              {{ evento.imagen }}<br>
              Descripción: {{ evento.descripcion }}<br>
          </li>
        </ul>

        <button @click="anteriorPagina" :disabled="paginaActual === 1">
          Anterior
        </button>
        
        <button @click="siguientePagina" :disabled="paginaActual === totalPaginas">
          Siguiente
        </button> 
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
  console.log(data)
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

