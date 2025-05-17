<template>
  <div class="contactos-container">
    <h2>Contactos</h2>
    <form @submit.prevent="guardarContacto">
      <input v-model="nuevoNombre" type="text" placeholder="Nombre" required />
      <input v-model="nuevoTelefono" type="text" placeholder="Teléfono" required />
      <button type="submit">{{ editando !== null ? 'Actualizar' : 'Agregar' }}</button>
      <button v-if="editando !== null" type="button" @click="cancelarEdicion">Cancelar</button>
    </form>
    <ul class="contactos-lista">
      <li v-for="(contacto, idx) in contactos" :key="idx">
        <span>{{ contacto.nombre }} - {{ contacto.telefono }}</span>
        <button @click="editarContacto(idx)">Editar</button>
        <button @click="eliminarContacto(idx)">Eliminar</button>
      </li>
    </ul>
    <button class="volver-btn" @click="$emit('volver')">Volver</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      contactos: [],
      nuevoNombre: '',
      nuevoTelefono: '',
      editando: null // índice del contacto que se está editando
    }
  },
  mounted() {
    this.cargarContactos();
  },
  methods: {
    cargarContactos() {
      const guardados = localStorage.getItem('vid_contactos');
      this.contactos = guardados ? JSON.parse(guardados) : [];
    },
    guardarContactos() {
      localStorage.setItem('vid_contactos', JSON.stringify(this.contactos));
    },
    guardarContacto() {
      if (this.editando !== null) {
        // Actualizar contacto existente
        this.contactos[this.editando] = {
          nombre: this.nuevoNombre,
          telefono: this.nuevoTelefono
        };
        this.editando = null;
      } else {
        // Agregar nuevo contacto
        this.contactos.push({
          nombre: this.nuevoNombre,
          telefono: this.nuevoTelefono
        });
      }
      this.guardarContactos();
      this.nuevoNombre = '';
      this.nuevoTelefono = '';
    },
    editarContacto(idx) {
      this.nuevoNombre = this.contactos[idx].nombre;
      this.nuevoTelefono = this.contactos[idx].telefono;
      this.editando = idx;
    },
    eliminarContacto(idx) {
      this.contactos.splice(idx, 1);
      this.guardarContactos();
      if (this.editando === idx) {
        this.cancelarEdicion();
      }
    },
    cancelarEdicion() {
      this.nuevoNombre = '';
      this.nuevoTelefono = '';
      this.editando = null;
    }
  }
}
</script>

<style>
.contactos-container {
  background: #C9A074;
  border-radius: 24px;
  box-shadow: 4px 4px 16px #8B5E3C;
  padding: 40px 32px;
  max-width: 500px;
  margin: 80px auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.contactos-container h2 {
  color: #8B5E3C;
  margin-bottom: 24px;
}
.contactos-container form {
  display: flex;
  gap: 8px;
  margin-bottom: 24px;
  flex-wrap: wrap;
}
.contactos-container input {
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #B6895B;
  font-size: 1rem;
}
.contactos-container button {
  background: #8B5E3C;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 8px 16px;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}
.contactos-container button:hover {
  background: #B6895B;
}
.contactos-lista {
  list-style: none;
  padding: 0;
  width: 100%;
}
.contactos-lista li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #B6895B;
  border-radius: 8px;
  margin-bottom: 8px;
  padding: 8px 12px;
  color: #fff;
}
.contactos-lista button {
  margin-left: 8px;
  background: #A47551;
}
.contactos-lista button:hover {
  background: #8B5E3C;
}
.volver-btn {
  background: #8B5E3C;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 12px 24px;
  font-size: 1.1rem;
  cursor: pointer;
  margin-top: 24px;
  transition: background 0.2s;
}
.volver-btn:hover {
  background: #B6895B;
}
</style>
