<template>
  <div class="login-container">
    <h2 v-if="mode === 'login'">Iniciar Sesión</h2>
    <h2 v-else>Registrarse</h2>
    <form @submit.prevent="mode === 'login' ? login() : register()">
      <input
        v-model="username"
        type="text"
        placeholder="Usuario"
        required
        autocomplete="username"
      />
      <input
        v-model="password"
        type="password"
        placeholder="Contraseña"
        required
        autocomplete="current-password"
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
export default {
  emits: ['login-success'],
  data() {
    return {
      mode: 'login', // o 'register'
      username: '',
      password: '',
      error: ''
    }
  },
  methods: {
    login() {
      const user = localStorage.getItem('vid_user');
      const pass = localStorage.getItem('vid_pass');
      if (this.username === user && this.password === pass) {
        this.$emit('login-success');
      } else {
        this.error = 'Usuario o contraseña incorrectos';
      }
    },
    register() {
      localStorage.setItem('vid_user', this.username);
      localStorage.setItem('vid_pass', this.password);
      this.mode = 'login';
      this.error = '¡Registro exitoso! Ahora inicia sesión.';
      this.username = '';
      this.password = '';
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
</style>
