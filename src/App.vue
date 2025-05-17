<template>
  <div v-if="pagina === 'landing'" class="landing">
    <header class="landing-header">
      <img src="/logo.png" alt="Logo" class="landing-logo" />
      <h1 class="landing-title">VID</h1>
    </header>
    <section class="landing-main">
      <div class="landing-img-box">
        <img src="/pareja.png" alt="Pareja" class="landing-img" />
      </div>
      <div class="landing-info">
        <h2 class="landing-subtitle">Cuidamos a quienes más te importan</h2>
        <p class="landing-desc">
          Con Vid, mantén a tus seres queridos seguros y siempre vigilados desde tu teléfono.
        </p>
        <button class="landing-btn" @click="pagina = 'login'">Iniciar App</button>
      </div>
    </section>
  </div>
  <Login v-else-if="pagina === 'login'" @login-success="pagina = 'transmision'" />
  <Transmision
    v-else-if="pagina === 'transmision'"
    @salir="pagina = 'landing'"
    @alertas="pagina = 'alertas'"
    @contactos="pagina = 'contactos'"
  />
  <Alertas v-else-if="pagina === 'alertas'" @volver="pagina = 'transmision'" />
  <Contactos v-else-if="pagina === 'contactos'" @volver="pagina = 'transmision'" />
</template>

<script>
import Transmision from './Transmision.vue'
import Login from './Login.vue'
import Alertas from './Alertas.vue'
import Contactos from './Contactos.vue'

export default {
  components: { Transmision, Login, Alertas, Contactos },
  data() {
    return {
      pagina: 'landing'
    }
  }
}
</script>

<style src="./index.css"></style>
<style>
.landing {
  background: #A47551;
  min-height: 100vh;
}
.landing-header {
  display: flex;
  align-items: center;
  gap: 24px;
  padding: 32px 0 24px 0;
  justify-content: center;
}
.landing-logo {
  width: 80px;
  height: 80px;
}
.landing-title {
  font-family: 'Georgia', serif;
  font-size: 4rem;
  font-weight: bold;
  letter-spacing: 16px;
  color: #8B5E3C;
  margin: 0;
}
.landing-main {
  background: #C9A074;
  border-radius: 0 0 0 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 48px;
  padding: 48px 0;
  flex-wrap: wrap;
}
.landing-img-box {
  background: none;
  box-shadow: 4px 4px 16px #8B5E3C;
  border-radius: 32px;
  width: 320px;
  height: 320px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.landing-img {
  width: 260px;
  height: 260px;
  object-fit: contain;
  border-radius: 24px;
}
.landing-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  max-width: 400px;
}
.landing-subtitle {
  font-size: 2rem;
  font-weight: bold;
  margin-bottom: 16px;
  color: #8B5E3C;
}
.landing-desc {
  font-size: 1.2rem;
  margin-bottom: 32px;
  color: #3E2723;
}
.landing-btn {
  background: #8B5E3C;
  color: #FFF;
  font-size: 2rem;
  font-weight: bold;
  border: none;
  border-radius: 24px;
  padding: 16px 40px;
  cursor: pointer;
  transition: background 0.2s;
  box-shadow: 2px 2px 8px #B6895B;
}
.landing-btn:hover {
  background: #B6895B;
}
</style>