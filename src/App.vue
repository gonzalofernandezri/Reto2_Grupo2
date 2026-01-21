<template>
  <header class="w-full"  >
     <div class="bg-purple-300 ">
      <h1>Mi Página Web</h1>

      <nav class="bg-purple-300">
        <ol class="flex gap-6">
          <li><router-link to="/juegos" class="text-blue-600 hoever:text-blue-800">Juegos</router-link></li> 
          <li><router-link to="/eventos" class="text-blue-600 hoever:text-blue-800">Eventos</router-link></li>
        </ol>
      </nav>

    <router-view />
    </div>
  </header>
</template>

<script>
import Juegos from './components/Juegos.vue';

</script>

<style>

</style>
