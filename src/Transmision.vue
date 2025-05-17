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
        <img 
          v-if="streamUrl" 
          :src="streamUrl" 
          alt="Transmisión en vivo" 
          class="video-stream"
          @error="handleStreamError" 
        />
        <div v-else class="connection-status">
          <span v-if="connectionError" class="error-message">{{ connectionError }}</span>
          <span v-else class="camera-text">Conectando a la cámara...</span>
        </div>
        <div class="camera-controls" v-if="streamUrl">
          <button class="control-btn" @click="reconnectStream">Reconectar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Transmision',
  data() {
    return {
      streamUrl: '',
      connectionError: '',
      streamEndpoint: '/video_feed',
      baseUrl: '' // URL base de ngrok (se configurará en mounted)
    }
  },
  mounted() {
    // Configurar URL del servidor
    this.configureServerUrl();
    // Iniciar la conexión al stream
    this.connectToStream();
  },
  methods: {
    configureServerUrl() {
      // Aquí configuras la URL de tu servidor Flask expuesto por ngrok
      // Debes cambiar esto por tu URL actual de ngrok cada vez que la inicies
      // o configurarla a través de variables de entorno
      this.baseUrl = import.meta.env.VITE_STREAM_SERVER_URL || 'https://tu-url-de-ngrok.ngrok.io';
    },
    connectToStream() {
      this.connectionError = '';
      this.streamUrl = `${this.baseUrl}${this.streamEndpoint}`;
      console.log('Conectando al stream en:', this.streamUrl);
    },
    handleStreamError(e) {
      console.error('Error al cargar el stream:', e);
      this.connectionError = 'Error de conexión con la cámara. Intenta reconectar.';
      this.streamUrl = '';
    },
    reconnectStream() {
      this.streamUrl = '';
      setTimeout(() => {
        this.connectToStream();
      }, 1000);
    }
  }
}
</script>

<style>
.container {
  display: flex;
  gap: 16px;
  height: 100vh;
  padding: 16px;
}

.sidebar {
  width: 240px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  height: 100%;
  position: relative;
}

.logo-box {
  width: 80px;
  height: 80px;
  background: #F8F0E3;
  border-radius: 16px;
  box-shadow: 2px 2px 8px #8B5E3C;
  margin: 0 auto;
  padding: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.menu-box {
  width: 240px;
  padding: 24px 16px 16px 16px;
  background: #F8F0E3;
  border-radius: 16px;
  box-shadow: 2px 2px 8px #8B5E3C;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  margin-top: 120px;
}

.menu-title {
  color: #8B5E3C;
  font-size: 1.5rem;
  margin-bottom: 16px;
  font-weight: bold;
}

.menu-btn {
  font-size: 1.25rem;
  width: 100%;
  padding: 12px 16px;
  margin: 8px 0;
  background: #D7BFA8;
  color: #4A2511;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s, transform 0.2s;
}

.menu-btn:hover {
  background: #C9A989;
  transform: translateY(-2px);
}

.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 16px;
  min-height: 100%;
}

.header-box {
  background: #D7BFA8;
  color: #4A2511;
  text-align: center;
  padding: 16px 0;
  font-size: 1.5rem;
  font-weight: bold;
  border-radius: 16px;
  box-shadow: 2px 2px 8px #8B5E3C;
  margin-bottom: 16px;
}

.camera-box {
  flex: 1;
  border: 4px solid #8B5E3C;
  border-radius: 16px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #000;
  position: relative;
}

.video-stream {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 12px;
}

.camera-text, .error-message {
  font-size: 1.5rem;
  color: #fff;
  text-shadow: 0 1px 4px #000;
}

.error-message {
  color: #ff5555;
}

.connection-status {
  padding: 16px;
  border-radius: 8px;
  background-color: rgba(0,0,0,0.5);
  text-align: center;
}

.camera-controls {
  position: absolute;
  bottom: 24px;
  right: 24px;
  display: flex;
  gap: 12px;
}

.control-btn {
  padding: 8px 16px;
  background: rgba(215, 191, 168, 0.8);
  color: #4A2511;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s;
  font-weight: bold;
}

.control-btn:hover {
  background: rgba(215, 191, 168, 1);
}

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
    margin: 0 auto;
    box-sizing: border-box;
  }
  .camera-text, .error-message {
    font-size: 1rem;
  }
  .camera-controls {
    position: static;
    margin-top: 8px;
    width: 100%;
    justify-content: center;
  }
  .control-btn {
    padding: 4px 8px;
    font-size: 0.85rem;
  }
  .video-stream {
    max-height: 100%;
    width: auto;
  }
}
</style>
