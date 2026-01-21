<template>
       <div>
        <h1>Juegos</h1>
        <ul>
            <li v-for="juego in juegos" :key="juego.id">
            {{ juego.titulo }} - {{ juego.genero }} - {{ juego.plataforma }}
            </li>
        </ul>
    </div>
</template>


<script setup>
import { onMounted, ref } from 'vue'


const juegos = ref([])


onMounted(async () => {
  const res = await fetch('/api/juegos_api.php')
  juegos.value = await res.json()
})
</script>