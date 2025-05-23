<template>
  <section class="pacto-verde-component certiffy-seccion">
    <!-- Banner Principal -->
    <ModuloBanner
      :tamanio-banner="'grande'"
      :imagen-fondo="backgroundImage"
      :usar-overlay="true"
      :titulo="title"
      :subtitulo="subtitle"
      :titulo-clase="bannerTitleClass"
      :subtitulo-clase="bannerSubtitleClass"
      :posicion-texto="'center'"
    />
    
    <!-- Categorías del módulo -->
    <div class="certiffy-contenedor q-py-xl q-px-md-none q-px-sm">
      <div class="row q-col-gutter-md q-col-gutter-lg-xl justify-center">
        <div 
          v-for="(category, index) in categories" 
          :key="index" 
          class="col-12 col-sm-12 col-md-6 col-lg-5 q-mb-md"
        >
          <ModuloTarjeta
            :titulo="category.title"
            :contenido="category.description"
            :accion-primaria="category.buttonText || 'Más información'"
            :ruta-accion-primaria="category.url"
            :color-accion-primaria="'primary'"
            :efecto-hover="true"
            :borde="true"
            tarjeta-clase="category-card h-100"
          >
            <template v-slot:contenido-personalizado>
              <q-list v-if="category.features && category.features.length" padding separator class="feature-list">
                <q-item v-for="(feature, idx) in category.features" :key="idx" class="feature-item">
                  <q-item-section avatar>
                    <q-icon name="check_circle" color="positive" size="sm" class="feature-icon" />
                  </q-item-section>
                  <q-item-section>
                    <q-item-label class="feature-text">{{ feature }}</q-item-label>
                  </q-item-section>
                </q-item>
              </q-list>
            </template>
          </ModuloTarjeta>
        </div>
      </div>
      
      <!-- Categorías de cabecera -->
      <div v-if="showHeaderCategories" class="header-categories q-mt-xl q-mb-md">
        <div class="row justify-center q-gutter-sm q-gutter-md-md text-center wrap">
          <div v-for="(cat, index) in headerCategories" :key="index" class="col-auto q-mb-sm">
            <q-chip
              color="primary"
              text-color="white"
              class="category-chip text-weight-medium q-px-md"
              :size="chipSize"
            >
              {{ cat }}
            </q-chip>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import ModuloBanner from './base/ModuloBanner.vue';
import ModuloTarjeta from './base/ModuloTarjeta.vue';

const props = defineProps({
  title: {
    type: String,
    required: true
  },
  subtitle: {
    type: String,
    required: true
  },
  backgroundImage: {
    type: String,
    required: true
  },
  categories: {
    type: Array,
    required: true
  },
  headerCategories: {
    type: Array,
    required: true
  },
  showHeaderCategories: {
    type: Boolean,
    required: true
  }
});

// Estado para el ancho de la ventana
const windowWidth = ref(window.innerWidth);

// Actualizar el ancho de la ventana cuando cambia el tamaño
const handleResize = () => {
  windowWidth.value = window.innerWidth;
};

// Agregar y eliminar el event listener para el cambio de tamaño
onMounted(() => {
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
});

// Clases responsivas para el título del banner
const bannerTitleClass = computed(() => {
  if (windowWidth.value < 600) {
    return 'text-h4 text-weight-bold text-white q-mb-md text-uppercase';
  } else if (windowWidth.value < 1024) {
    return 'text-h3 text-weight-bold text-white q-mb-md text-uppercase';
  } else {
    return 'text-h3 text-weight-bold text-white text-uppercase';
  }
});

// Clases responsivas para el subtítulo del banner
const bannerSubtitleClass = computed(() => {
  if (windowWidth.value < 600) {
    return 'text-subtitle2 text-white q-mb-sm';
  } else {
    return 'text-subtitle1 text-white q-mb-md';
  }
});

// Tamaño responsivo para los chips
const chipSize = computed(() => {
  return windowWidth.value < 600 ? 'md' : 'lg';
});
</script>

<style scoped>
.pacto-verde-component.certiffy-seccion {
  width: 100%;
  background-color: var(--q-grey-1);
}

.pacto-verde-component .certiffy-contenedor {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}

.pacto-verde-component .category-card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.pacto-verde-component .category-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.pacto-verde-component .category-chip {
  transition: transform 0.2s ease;
}

.pacto-verde-component .category-chip:hover {
  transform: scale(1.05);
}

/* Estilos responsivos para dispositivos móviles */
@media (max-width: 599px) {
  .pacto-verde-component .feature-text {
    font-size: 0.9rem !important;
  }
  
  .pacto-verde-component .feature-item {
    min-height: 40px !important;
    padding: 8px 0 !important;
  }
  
  .pacto-verde-component .feature-icon {
    font-size: 1rem !important;
  }
  
  .pacto-verde-component .header-categories {
    margin-top: 2rem !important;
  }
  
  .pacto-verde-component .certiffy-contenedor {
    padding: 0 0.5rem;
  }
}

/* Ajustes para tablets */
@media (min-width: 600px) and (max-width: 1023px) {
  .pacto-verde-component .feature-text {
    font-size: 1rem !important;
  }
  
  .pacto-verde-component .feature-item {
    min-height: 48px !important;
  }
}
</style>