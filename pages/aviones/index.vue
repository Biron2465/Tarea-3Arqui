<template>
  <div class="container my-5">
    <Header />
    <div class="text-center mb-5">
      <h1 class="display-3 font-weight-bold text-primary">Lista de Aviones</h1>
    </div>
    <main>
      <div class="row g-4">
        <li v-for="avion in aviones" :key="avion.modelo" class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 shadow-sm border-light">
            <div class="card-body text-center p-4">
              <h5 class="card-title text-dark font-weight-bold mb-4">{{ avion.modelo }}</h5>
              <nuxt-link :to="{ path: `/aviones/${generateSlug(avion.modelo)}` }" class="btn btn-info mb-3">
                Ver detalles
              </nuxt-link>
              <div>
                <button
                  class="btn btn-warning snipcart-add-item"
                  :data-item-id="'avion-' + avion.modelo"
                  :data-item-name="avion.modelo"
                  data-item-price="150.00"
                  :data-item-url="`/aviones/${generateSlug(avion.modelo)}`"
                  data-item-description="Descripción del avión"
                >
                  Comprar
                </button>
              </div>
            </div>
          </div>
        </li>
      </div>
    </main>
    <Footer />
    <div
      id="snipcart"
      hidden
      data-api-key="MWVmZGE4NjEtYTdlNC00ZmIzLTk2MTEtMmIxYzA2MmU1N2EzNjM4NjY3MTE4MjU1ODkxNjE0"
      data-config-modal-style="side"
    ></div>
  </div>
</template>

<script setup>
import Header from '@/components/header.vue';
import Footer from '@/components/footer.vue';
import { ref, onMounted } from 'vue';

const aviones = ref([]);

const importAviones = async () => {
  const modules = import.meta.glob('@/data/aviones/*.json');
  const avionPromises = Object.values(modules).map((module) => module());
  const avionData = await Promise.all(avionPromises);
  aviones.value = avionData.map(data => data.default || data);
};

const generateSlug = (text) => {
  return text
    .toString()
    .toLowerCase()
    .replace(/\s+/g, '-')           // Reemplaza espacios con -
    .replace(/[^\w\-]+/g, '')       // Elimina caracteres no válidos
    .replace(/\-\-+/g, '-')         // Reemplaza múltiples - con uno solo
    .replace(/^-+/, '')             // Elimina - al inicio
    .replace(/-+$/, '');            // Elimina - al final
};

onMounted(() => {
  importAviones();
});
</script>

<style scoped>
/* Espaciado y estilo de las tarjetas */
li {
  list-style: none;
}

.card {
  transition: transform 0.3s, box-shadow 0.3s;
  border-color: #dee2e6;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}

.card-title {
  color: #343a40;
}

.btn-info {
  background-color: #17a2b8;
  border-color: #17a2b8;
  color: #fff;
  transition: background-color 0.3s;
}

.btn-info:hover {
  background-color: #138496;
  border-color: #117a8b;
}

.btn-warning {
  background-color: #ffc107;
  border-color: #ffc107;
  color: #343a40;
  transition: background-color 0.3s;
}

.btn-warning:hover {
  background-color: #e0a800;
  border-color: #d39e00;
}
</style>
