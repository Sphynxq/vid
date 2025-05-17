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
    padding: 4px;
    gap: 4px;
    min-height: 100vh;
    height: auto;
  }
  .sidebar {
    width: 100%;
    flex-direction: row;
    align-items: flex-start;
    justify-content: center;
    gap: 4px;
    height: auto;
    position: static;
    padding: 0;
    margin-bottom: 4px;
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
    gap: 4px;
    flex: 1 1 auto;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    min-height: unset;
    height: auto;
  }
  .header-box {
    font-size: 1.1rem;
    padding: 8px 0;
    border-radius: 8px;
    margin-bottom: 4px;
  }
  .camera-box {
    height: 48vw;
    max-height: 180px;
    aspect-ratio: 16 / 9;
    width: 100%;
    border-width: 2px;
    border-radius: 8px;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #000;
    margin: 0 auto;
    box-sizing: border-box;
  }
  .camera-text {
    font-size: 1rem;
    color: #fff;
    text-shadow: 0 1px 4px #000;
  }
}
</style>
