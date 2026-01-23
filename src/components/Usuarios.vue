<template>
  <div class="justify-center flex flex-col items-center min-h-screen space-y-4">
    <h2>Registro de Usuario</h2>
    <form @submit.prevent="registrarUsuario">
      <div>
        <label>Usuario:</label>
        <input type="text" v-model="usuario" />
      </div>
      <div>
        <label>Email:</label>
        <input type="text" v-model="email" />
      </div>
      <div>
        <label>Contraseña:</label>
        <input type="password" v-model="contraseña" />
      </div>
<button type="submit">Registrar</button>
<ul v-if="errores.length" style="color:red">
        <li v-for="(err, i) in errores" :key="i">{{ err }}</li>
      </ul>
      <p v-if="correcto" style="color:green">{{ correcto }}</p>
    </form>
  </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
  data() {
    return {
      usuario: '',
      email: '',
      contraseña: '',
      errores: [],
      correcto: ''
    }
  },
methods: {
  mostrarAlerta(titulo) {
Swal.fire({
  title: titulo,
  background: '#1e1e2f', // gris oscuro
  color: '#f5f5f5',       // texto claro
  confirmButtonColor: '#4f46e5' // azul brillante
})


  },

  registrarUsuario() {
    if (!this.usuario) {
      this.mostrarAlerta('El nombre de usuario es obligatorio')
      return
    }
    if (!this.email) {
      this.mostrarAlerta('El email es obligatorio')
      return
    }
    if (!this.contraseña) {
      this.mostrarAlerta('La contraseña es obligatoria')
      return
    }
    this.mostrarAlerta('Te has registrado correctamente')
  }
}
}

</script>



