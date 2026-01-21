<template>
    <div>
        <select v-model="tipo" @change="cargarEventos">
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
    </div>
</template>


<script setup>
import { ref, onMounted } from 'vue'

const eventos = ref([])
const tipo = ref('')

async function cargarEventos() {
  const params = new URLSearchParams()

  if (tipo.value) {
    params.append('tipo', tipo.value)
  }

  const res = await fetch(`/api/eventos_api.php?${params.toString()}`)
  eventos.value = await res.json()
}

onMounted(cargarEventos)
</script>

