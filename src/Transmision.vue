<template>
  <div class="container">
    <div class="sidebar">
      <div class="logo-box">
        <img src="/logo.png" alt="Logo" class="logo-img" />
      </div>
      <div class="menu-box">
        <div class="menu-title">Menú</div>
        <button class="menu-btn" @click="$emit('alertas')">Alertas</button>
        <button class="menu-btn" @click="$emit('contactos')">Contactos</button>
        <button class="menu-btn" @click="$emit('salir')">Salir</button>
      </div>
    </div>
    <div class="main-content">
      <div class="header-box">Transmision en tiempo real</div>
      <div class="camera-box">
        <span class="camera-text">Vista principal</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Transmision'
}
</script>
