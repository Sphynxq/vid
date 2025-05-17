<template>
  <div class="alertas-container">
    <h2>Alertas</h2>
    <div v-if="alertas.length === 0" class="alertas-vacio">
      No hay alertas por el momento.
    </div>
    <ul v-else>
      <li v-for="(alerta, idx) in alertas" :key="idx">
        {{ alerta.mensaje }}
      </li>
    </ul>
    <button class="volver-btn" @click="$emit('volver')">Volver</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      alertas: [] // Aquí se llenará con datos del backend en el futuro
    }
  }
}
</script>

<style>
.alertas-container {
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
.alertas-container h2 {
  color: #8B5E3C;
  margin-bottom: 24px;
}
.alertas-vacio {
  color: #3E2723;
  font-size: 1.2rem;
  margin-bottom: 16px;
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
