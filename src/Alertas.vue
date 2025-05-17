<template>
  <div class="alertas-container">
    <h2>Alertas de Caídas</h2>
    
    <div v-if="loading" class="loading">
      Cargando alertas...
    </div>
    
    <div v-else-if="error" class="error-message">
      {{ error }}
    </div>
    
    <div v-else-if="caidas.length === 0" class="alertas-vacio">
      No hay alertas de caídas registradas.
    </div>
    
    <div v-else class="caidas-lista">
      <div v-for="caida in caidas" :key="caida._id" class="caida-item">
        <div class="caida-header">
          <h3>{{ caida.tipo }}</h3>
          <span class="fecha">{{ formatearFecha(caida.fecha) }}</span>
        </div>
        
        <div class="caida-info">
          <p><strong>Ubicación:</strong> {{ caida.ubicacion }}</p>
          <p><strong>Descripción:</strong> {{ caida.descripcion }}</p>
          <p v-if="caida.severidad"><strong>Severidad:</strong> {{ caida.severidad }}</p>
        </div>
        
        <div v-if="caida.imagenes_info && caida.imagenes_info.length > 0" class="caida-imagenes">
          <h4>Imágenes:</h4>
          <div class="galeria-imagenes">
            <div v-for="imagen in caida.imagenes_info" :key="imagen.id" class="imagen-item">
              <img :src="imagen.url_thumbnail" :alt="imagen.descripcion || 'Imagen de caída'" 
                  @click="mostrarImagenCompleta(imagen)" />
              <p v-if="imagen.descripcion" class="imagen-descripcion">{{ imagen.descripcion }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <div v-if="imagenSeleccionada" class="modal-imagen" @click="cerrarModal">
      <div class="modal-contenido">
        <img :src="imagenSeleccionada.url_cloudinary" :alt="imagenSeleccionada.descripcion || 'Imagen de caída'" />
        <p v-if="imagenSeleccionada.descripcion">{{ imagenSeleccionada.descripcion }}</p>
        <button class="cerrar-btn" @click.stop="cerrarModal">Cerrar</button>
      </div>
    </div>
    
    <button class="volver-btn" @click="$emit('volver')">Volver</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      caidas: [],
      loading: false,
      error: null,
      imagenSeleccionada: null
    }
  },
  mounted() {
    this.cargarCaidas();
  },
  methods: {
    async cargarCaidas() {
      this.loading = true;
      this.error = null;
      
      try {
        const response = await axios.get('https://backvid.onrender.com/caidas');
        this.caidas = response.data.caidas || [];
        
        // Si hay caídas pero no tienen información de imágenes, cargamos las imágenes
        for (const caida of this.caidas) {
          if (!caida.imagenes_info || caida.imagenes_info.length === 0) {
            await this.cargarImagenesDeCaida(caida);
          }
        }
      } catch (err) {
        console.error('Error al cargar caídas:', err);
        this.error = 'No se pudieron cargar las alertas de caídas. Inténtelo nuevamente más tarde.';
      } finally {
        this.loading = false;
      }
    },
    
    async cargarImagenesDeCaida(caida) {
      try {
        const response = await axios.get(`https://backvid.onrender.com/imagenes?caida_id=${caida._id}`);
        if (response.data && response.data.imagenes) {
          // Añadimos la información de imágenes a la caída
          caida.imagenes_info = response.data.imagenes;
        }
      } catch (err) {
        console.error(`Error al cargar imágenes para caída ${caida._id}:`, err);
      }
    },
    
    formatearFecha(fechaStr) {
      if (!fechaStr) return 'Fecha desconocida';
      const fecha = new Date(fechaStr);
      if (isNaN(fecha.getTime())) {
        // Si no se puede convertir como fecha ISO, intentamos con el formato original
        return fechaStr;
      }
      return fecha.toLocaleString('es-ES', {
        day: '2-digit',
        month: '2-digit',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      });
    },
    
    mostrarImagenCompleta(imagen) {
      this.imagenSeleccionada = imagen;
    },
    
    cerrarModal() {
      this.imagenSeleccionada = null;
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
  max-width: 800px;
  margin: 80px auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.alertas-container h2 {
  color: #8B5E3C;
  margin-bottom: 24px;
}

.loading, .alertas-vacio, .error-message {
  color: #3E2723;
  font-size: 1.2rem;
  margin-bottom: 16px;
  text-align: center;
}

.error-message {
  color: #b71c1c;
}

.caidas-lista {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.caida-item {
  background: #F8F0E3;
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 2px 8px rgba(139, 94, 60, 0.2);
  width: 100%;
}

.caida-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.caida-header h3 {
  color: #8B5E3C;
  margin: 0;
  font-size: 1.2rem;
}

.fecha {
  color: #8B5E3C;
  font-size: 0.9rem;
}

.caida-info {
  margin-bottom: 15px;
}

.caida-info p {
  margin: 5px 0;
  color: #3E2723;
}

.caida-imagenes h4 {
  color: #8B5E3C;
  margin-bottom: 10px;
  font-size: 1.1rem;
}

.galeria-imagenes {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.imagen-item {
  width: 100px;
  margin-bottom: 10px;
}

.imagen-item img {
  width: 100%;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.3s;
}

.imagen-item img:hover {
  transform: scale(1.05);
}

.imagen-descripcion {
  font-size: 0.8rem;
  color: #3E2723;
  text-align: center;
  margin-top: 4px;
}

.modal-imagen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-contenido {
  background: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 80%;
  max-height: 80%;
  position: relative;
  overflow: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.modal-contenido img {
  max-width: 100%;
  max-height: 70vh;
  object-fit: contain;
  margin-bottom: 10px;
}

.modal-contenido p {
  margin: 10px 0;
  color: #3E2723;
}

.cerrar-btn {
  padding: 8px 16px;
  background: #8B5E3C;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9rem;
  margin-top: 10px;
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

.volver-btn:hover, .cerrar-btn:hover {
  background: #B6895B;
}

@media (max-width: 600px) {
  .alertas-container {
    padding: 16px 12px;
    max-width: 100%;
    border-radius: 10px;
    margin: 16px auto;
  }
  
  .alertas-container h2 {
    font-size: 1.3rem;
    margin-bottom: 16px;
  }
  
  .caida-header {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .fecha {
    margin-top: 4px;
  }
  
  .imagen-item {
    width: 80px;
  }
  
  .imagen-item img {
    height: 80px;
  }
  
  .modal-contenido {
    max-width: 95%;
    padding: 12px;
  }
  
  .loading, .alertas-vacio, .error-message {
    font-size: 1rem;
  }
  
  .volver-btn {
    font-size: 1rem;
    padding: 8px 16px;
    margin-top: 16px;
  }
}
</style>
