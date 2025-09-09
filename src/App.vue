<script setup>

import { ref, reactive, onMounted, watch } from 'vue'
import { db } from './data/guitarras'
import Guitarra from './components/Guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const state = reactive({
  guitarras: []
})



const guitarras = ref([])
const carrito = ref([])
const guitarra = ref({})

//guitarras.value = db;

watch(carrito, () => {
  guardarLocalStorage();
},
  {
    deep: true
  })


onMounted(() => {
  console.log("componente listo");
  guitarras.value = db;
  state.guitarras = db;
  guitarra.value = db[3];


  const carritoStorage = localStorage.getItem("carrito");
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage);
  }

});

const guardarLocalStorage = () => {
  localStorage.setItem("carrito", JSON.stringify(carrito.value));
}

const agregarCarrito = (guitarra) => {

  const existeCarrito = carrito.value.findIndex(p => p.id == guitarra.id);
  if (existeCarrito != -1) {
    if (carrito.value[index].cantidad >= 5) {
      return;
    }
    carrito.value[existeCarrito].cantidad++;
  }
  else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }

  console.log(carrito)
}

const aumentarCantidadCarrito = (id) => {
  console.log(id);
  const index = carrito.value.findIndex(p => p.id == id);
  if (carrito.value[index].cantidad >= 5) {
    return;
  }
  carrito.value[index].cantidad++;
}

const disminuirCantidadCarrito = (id) => {
  console.log(id)
  const index = carrito.value.findIndex(p => p.id == id);
  if (carrito.value[index].cantidad <= 1) {
    return;
  }
  carrito.value[index].cantidad--;
}

const eliminarProductoCarrito = (id) => {
  //  const index = carrito.value.findIndex(p=>p.id == id);
  //  carrito.value.splice(index, 1);
  carrito.value = carrito.value.filter(x => x.id != id)
}

const vaciarCarrito = () => {
  carrito.value = [];
}

</script>

<template>


  <Header v-bind:carrito="carrito" :guitarra="guitarra" @disminuir-cantidad-carrito="disminuirCantidadCarrito"
    @aumentar-cantidad-carrito="aumentarCantidadCarrito" @eliminar-producto-carrito="eliminarProductoCarrito"
    @agregar-carrito="agregarCarrito" @vaciar-carrito="vaciarCarrito"></Header>

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra v-for="guitarra in guitarras" v-bind:guitarra="guitarra" @agregar-carrito="agregarCarrito"></Guitarra>

    </div>
  </main>


  <Footer />
</template>

<style scoped></style>
