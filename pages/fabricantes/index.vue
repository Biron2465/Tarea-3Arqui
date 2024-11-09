<template>
  <div class="container my-5">
    <Header />
    <div class="text-center mb-5">
      <h1 class="display-3 font-weight-bold text-primary">Lista de Fabricantes</h1>
    </div>
    <main>
      <div class="row g-4">
        <li v-for="fabricante in fabricantes" :key="fabricante.nombre" class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 shadow-sm border-light">
            <div class="card-body text-center p-4">
              <h5 class="card-title text-dark font-weight-bold mb-4">{{ fabricante.nombre }}</h5>
              <nuxt-link :to="`/fabricantes/${fabricante.nombre}`" class="btn btn-info mb-3">
                Ver detalles
              </nuxt-link>
              <div>
                <button
                  class="btn btn-warning snipcart-add-item"
                  :data-item-id="'fabricante-' + fabricante.nombre"
                  :data-item-name="fabricante.nombre"
                  data-item-price="200.00"
                  :data-item-url="`/fabricantes/${fabricante.nombre}`"
                  data-item-description="Descripción del fabricante"
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

const fabricantes = ref([]);

const importFabricantes = async () => {
  const modules = import.meta.glob('@/data/fabricantes/*.json');
  const fabricantePromises = Object.values(modules).map((module) => module());
  const fabricanteData = await Promise.all(fabricantePromises);
  fabricantes.value = fabricanteData.flat();
};

onMounted(() => {
  importFabricantes();
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
