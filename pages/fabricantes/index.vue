<template>
  <div class="container my-5">
    <Header />
    <div class="text-center mb-4">
      <h1 class="display-4">Lista de Fabricantes</h1>
    </div>
    <main>
      <ul class="list-unstyled row">
        <li v-for="fabricante in fabricantes" :key="fabricante.nombre" class="col-md-4 mb-4">
          <div class="card h-100">
            <div class="card-body text-center">
              <h5 class="card-title">{{ fabricante.nombre }}</h5>
              <nuxt-link :to="`/fabricantes/${fabricante.nombre}`" class="btn btn-link">
                Ver detalles
              </nuxt-link>
              <button
                class="btn btn-primary snipcart-add-item"
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
        </li>
      </ul>
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
/* Puedes agregar estilos personalizados aquí si lo necesitas */
</style>
