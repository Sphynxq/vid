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

<style>
@media (max-width: 600px) {
  .container {
    flex-direction: column;
    padding: 8px;
    gap: 8px;
  }
  .sidebar {
    width: 100%;
    flex-direction: row;
    align-items: flex-start;
    justify-content: center;
    gap: 8px;
    height: auto;
    position: static;
    padding: 0;
  }
  .logo-box {
    width: 40px;
    height: 40px;
    margin: 0 8px 0 0;
    padding: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .logo-img {
    width: 32px;
    height: 32px;
  }
  .menu-box {
    width: auto;
    min-width: 0;
    padding: 8px 4px 8px 4px;
    border-radius: 8px;
    box-shadow: 2px 2px 6px #8B5E3C;
    display: flex;
    flex-direction: row;
    align-items: center;
    position: static;
    top: auto;
    left: auto;
    transform: none;
    margin: 0;
    gap: 4px;
  }
  .menu-title {
    display: none;
  }
  .menu-btn {
    font-size: 0.95rem;
    width: auto;
    padding: 6px 10px;
    margin: 0 2px;
    border-radius: 6px;
    flex: 1 1 auto;
    min-width: 0;
  }
  .main-content {
    gap: 8px;
  }
  .header-box {
    font-size: 1.5rem;
    padding: 12px 0;
    border-radius: 10px;
  }
  .camera-box {
    min-height: unset;
    height: 40vw;
    max-height: 220px;
    aspect-ratio: 16 / 9;
    width: 100%;
    border-width: 3px;
    border-radius: 8px;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #000; /* Simula fondo de video */
  }
  .camera-text {
    font-size: 1.1rem;
    color: #fff;
    text-shadow: 0 1px 4px #000;
  }
}
</style>
