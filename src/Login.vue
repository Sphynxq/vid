<template>
  <div class="login-container">
    <h2 v-if="mode === 'login'">Iniciar Sesión</h2>
    <h2 v-else>Registrarse</h2>
    <form @submit.prevent="mode === 'login' ? login() : register()">
      <input
        v-if="mode === 'register'"
        v-model="nombre"
        type="text"
        placeholder="Nombre"
        required
        autocomplete="name"
        id="nombre"
        name="nombre"
      />
      <input
        v-model="email"
        type="email"
        placeholder="Correo electrónico"
        required
        autocomplete="email"
        id="email"
        name="email"
      />
      <input
        v-model="password"
        type="password"
        placeholder="Contraseña"
        required
        autocomplete="current-password"
        id="password"
        name="password"
      />
      <input
        v-if="mode === 'register'"
        v-model="telefono"
        type="tel"
        placeholder="Teléfono"
        required
        autocomplete="tel"
        id="telefono"
        name="telefono"
      />
      <button type="submit">
        {{ mode === 'login' ? 'Iniciar Sesión' : 'Registrarse' }}
      </button>
      <p v-if="error" class="error">{{ error }}</p>
    </form>
    <div class="login-switch">
      <span v-if="mode === 'login'">
        ¿No tienes cuenta?
        <a href="#" @click.prevent="mode = 'register'; error = ''">Regístrate</a>
      </span>
      <span v-else>
        ¿Ya tienes cuenta?
        <a href="#" @click.prevent="mode = 'login'; error = ''">Inicia sesión</a>
      </span>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  emits: ['login-success'],
  data() {
    return {
      mode: 'login', // o 'register'
      nombre: '',
      email: '',
      password: '',
      telefono: '',
      error: ''
    }
  },
  methods: {
    async login() {
      try {
        // Crear un objeto de datos directamente sin referenciar elementos del DOM
        const loginData = {
          email: this.email,
          password: this.password
        };
        
        console.log('Intentando iniciar sesión con:', { email: this.email });
        
        const response = await axios.post('https://backvid.onrender.com/login', loginData);
        const data = response.data;
        
        console.log('Respuesta del servidor:', data);
        
        if ((data && data.success) || (data && data.mensaje)) {
          this.$emit('login-success');
        } else {
          this.error = (data && data.message) || (data && data.mensaje) || 'Usuario o contraseña incorrectos';
        }
      } catch (err) {
        console.error('Error durante el inicio de sesión:', err);
        this.error = err.response?.data?.message || 'Error de conexión con el servidor';
      }
    },
    async register() {
      // Validaciones básicas antes de enviar al backend
      if (!this.nombre || !this.email || !this.password || !this.telefono) {
        this.error = 'Todos los campos son obligatorios';
        console.log('Validación fallida: campos vacíos', {
          nombre: this.nombre,
          email: this.email,
          password: this.password,
          telefono: this.telefono
        });
        return;
      }
      if (this.password.length < 6) {
        this.error = 'La contraseña debe tener al menos 6 caracteres';
        console.log('Validación fallida: contraseña corta', this.password);
        return;
      }
      if (!/^\d{7,15}$/.test(this.telefono)) {
        this.error = 'El teléfono debe tener entre 7 y 15 dígitos';
        console.log('Validación fallida: teléfono incorrecto', this.telefono);
        return;
      }
      try {
        // Crear objeto de datos directamente
        const registroData = {
          nombre: this.nombre,
          email: this.email,
          password: this.password,
          telefono: this.telefono
        };

        console.log('Enviando datos al backend:', registroData);
        
        const response = await axios.post('https://backvid.onrender.com/registro', registroData);
        const data = response.data;
        console.log('Respuesta del backend (registro):', data);
        if ((data && data.success) || (data && data.mensaje)) {
          // Intentar login automáticamente después del registro
          try {
            console.log('Intentando login automático...');
            const loginData = {
              email: this.email,
              password: this.password
            };
            const loginResponse = await axios.post('https://backvid.onrender.com/login', loginData);
            const loginResponseData = loginResponse.data;
            console.log('Respuesta del backend (login):', loginResponseData);
            if ((loginResponseData && loginResponseData.success) || (loginResponseData && loginResponseData.mensaje)) {
              this.$emit('login-success');
            } else {
              this.error = (loginResponseData && loginResponseData.message) || (loginResponseData && loginResponseData.mensaje) || 'No se pudo iniciar sesión después del registro';
              console.log('Error en login automático:', this.error);
            }
          } catch (loginErr) {
            console.error('Error durante el login automático:', loginErr);
            this.error = loginErr.response?.data?.message || 'Error al iniciar sesión después del registro';
            console.log('Excepción en login automático:', loginErr);
          }
          // Limpiar campos
          this.nombre = '';
          this.email = '';
          this.password = '';
          this.telefono = '';
        } else {
          this.error = (data && data.message) || (data && data.mensaje) || 'No se pudo registrar';
          console.log('Error en registro:', this.error, data);
        }
      } catch (err) {
        console.error('Error durante el registro:', err);
        this.error = err.response?.data?.message || 'Error de conexión con el servidor';
        console.log('Excepción en registro:', err, err.response);
      }
    }
  }
}
</script>

<style>
.login-container {
  background: #C9A074;
  border-radius: 24px;
  box-shadow: 4px 4px 16px #8B5E3C;
  padding: 40px 32px;
  max-width: 350px;
  margin: 80px auto;
  display: flex;  
  flex-direction: column;
  align-items: center;
}
.login-container h2 {
  color: #8B5E3C;
  margin-bottom: 24px;
}
.login-container form {
  display: flex;
  flex-direction: column;
  width: 100%;
}
.login-container input {
  margin-bottom: 16px;
  padding: 12px;
  border-radius: 8px;
  border: 1px solid #B6895B;
  font-size: 1.1rem;
}
.login-container button {
  background: #8B5E3C;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 12px;
  font-size: 1.1rem;
  cursor: pointer;
  margin-bottom: 8px;
  transition: background 0.2s;
}
.login-container button:hover {
  background: #B6895B;
}
.login-switch {
  margin-top: 12px;
  color: #3E2723;
  font-size: 1rem;
}
.login-switch a {
  color: #8B5E3C;
  cursor: pointer;
  text-decoration: underline;
}
.error {
  color: #b71c1c;
  margin-bottom: 8px;
  font-size: 1rem;
}
@media (max-width: 600px) {
  .login-container {
    padding: 16px 4px;
    max-width: 98vw;
    border-radius: 10px;
    margin: 32px auto;
  }
  .login-container h2 {
    font-size: 1.3rem;
    margin-bottom: 16px;
  }
  .login-container input {
    font-size: 1rem;
    padding: 8px;
  }
  .login-container button {
    font-size: 1rem;
    padding: 8px;
    border-radius: 6px;
  }
  .login-switch {
    font-size: 0.95rem;
  }
}
</style>