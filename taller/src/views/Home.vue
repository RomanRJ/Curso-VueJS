
<template>
  <div class="container py-4">
    <nav class="navbar navbar-light bg-light rounded-3 mb-4">
      <div class="container-fluid">
        <span class="navbar-brand mb-0 h4">Curso Vue</span>
      </div>
    </nav>

    <div class="row g-4">
      <!-- Generador de Gatos -->
      <div class="col-lg-6">
        <div class="card shadow-sm">
          <div class="card-body">
            <h2 class="card-title h5">Generador de Gatos</h2>
            <p class="card-text text-muted">
              Haz clic en el botón para obtener una imagen aleatoria de gato desde la API.
            </p>

            <button
              class="btn btn-primary"
              :class="{ 'disabled': loading }"
              @click="gato"
              :disabled="loading"
            >
              <span v-if="loading" class="spinner-border spinner-border-sm me-2" role="status" aria-hidden="true"></span>
              {{ loading ? 'Cargando...' : 'Gato' }}
            </button>

            <div class="mt-3">
              <div v-if="imageUrl" class="text-center">
                <img :src="imageUrl" alt="cat" class="img-fluid rounded shadow-sm" />
              </div>
              <div v-else class="text-center text-muted">
                <p class="mb-0">Aún no hay imagen. Haz clic en "Gato" para mostrar una.</p>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Reactividad - ejemplos -->
      <div class="col-lg-6">
        <div class="card shadow-sm h-100">
          <div class="card-body">
            <h2 class="card-title h5">Ejemplos de reactividad</h2>
            <p class="text-muted mb-3">Prueba botones, inputs y observa cómo Vue actualiza la vista.</p>

            <!-- Caja de color -->
            <div class="mb-3">
              <div class="d-flex gap-2 mb-2">
                <button class="btn btn-sm btn-outline-primary" id="boton" @click="setColor('#0d6efd')">Azul</button>
                <button class="btn btn-sm btn-outline-success" @click="setColor('#198754')">Verde</button>
                <button class="btn btn-sm btn-outline-danger" @click="setColor('#dc3545')">Rojo</button>
                <button class="btn btn-sm btn-outline-secondary" @click="setColor('#f0f0f0')">Reset</button>
                <button class="btn btn-sm btn-outline-dark" @click="box.visible = !box.visible">
                  {{ box.visible ? 'Ocultar' : 'Mostrar' }}
                </button>
              </div>

              <div v-if="box.visible" class="p-3 rounded border"  :style="{ background: box.color }">
                <strong>Recuadro reactivo</strong>
                <p class="mb-0 small text-muted">Su color cambia con los botones.</p>
              </div>
              <div v-else class="p-3 rounded border"  :style="{ background: box.color }">
                <strong>Soy invisible</strong>
                <p class="mb-0 small text-muted">Desapareció.</p>
              </div>
            </div>

            <hr />

            <!-- Input y saludo -->
            <div class="mb-3">
              <label class="form-label">Nombre</label>
              <input v-model="texto" class="form-control" placeholder="Escribe tu nombre" />
              <p class="mt-2">Salida: <strong>Hola! {{ texto || 'amigo' }} </strong></p>
            </div>

            <hr />

            <!-- Contador -->
            <div class="mb-3 d-flex align-items-center gap-2">
              <button class="btn btn-outline-secondary" @click="counter--">-</button>
              <div><strong>{{ counter }}</strong></div>
              <button class="btn btn-outline-secondary" @click="counter++">+</button>
              <button class="btn btn-sm btn-outline-danger ms-2" @click="counter = 0">Reset</button>
            </div>

            <hr />

            <!-- Lista dinámica -->
            <div>
              <label class="form-label">Lista (reactiva)</label>
              <div class="input-group mb-2">
                <input v-model="newItem" class="form-control" placeholder="Nuevo item" @keyup.enter="addItem" />
                <button class="btn btn-primary" @click="addItem">Agregar</button>
              </div>

              <ul class="list-group">
                <li v-for="(i, idx) in list" :key="idx" class="list-group-item d-flex justify-content-between align-items-center">
                  {{ i }}
                  <button class="btn btn-sm btn-outline-danger" @click="removeItem(idx)">Eliminar</button>
                </li>
              </ul>
            </div>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  data() {
    return {
      imageUrl: '',
      loading: false,
      // reactividad
      box: {
        color: '#f0f0f0',
        visible: true
      },
      texto: '',
      counter: 0,
      list: ['Elemento 1', 'Elemento 2'],
      newItem: ''
    };
  },
  methods: {
    async gato() {
      try {
        this.loading = true;
        // pedir JSON para obtener la url (asegura respuesta JSON)
        const response = await axios.get('https://cataas.com/cat?json=true');
        const url = response.data && response.data.url ? response.data.url : '';
        this.imageUrl = url.startsWith('http') ? url : (url ? `https://cataas.com${url}` : '');
        console.log(response);
      } catch (err) {
        console.error('Error obteniendo la imagen:', err);
      } finally {
        this.loading = false;
      }
    },
    setColor(c) {
      this.box.color = c;
    },
    addItem() {
      if (!this.newItem) return;
      this.list.push(this.newItem);
      this.newItem = '';
    },
    removeItem(i) {
      this.list.splice(i, 1);
    }
  }
}
</script>

<style scoped>
.cat-wrap img,
.card img,
img { max-width: 100%; height: auto; display: block; margin-top: 1rem; }
</style>
