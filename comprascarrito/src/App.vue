<script setup>
// logica

  import { ref, reactive, onMounted, watch } from 'vue'
  import {db} from './data/guitarras.js'
  import Guitarra from './components/Guitarra.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  // const state = reactive({
  //   guitarras: db
  // })

  // console.log(state.guitarras);

  const guitarras = ref([]);
  const carrito = ref([]);
  const guitarra = ref({});

  watch(carrito, () =>{
    guardarLocalStorage();
  }, {
    deep: true
  });

  onMounted(() => {
    guitarras.value = db;
    guitarra.value = db[3];

    const carritoStorage = localStorage.getItem('carrito');
    if(carritoStorage){
      carrito.value = JSON.parse(carritoStorage);
    }
    
  })


  const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id);
    
    if(existeCarrito >= 0){
      carrito.value[existeCarrito].cantidad++;
    }else{
      guitarra.cantidad = 1;
      carrito.value.push(guitarra);
    }
  }

  const decrementarCantidad = (id) =>{
    const index = carrito.value.findIndex(producto => producto.id === id);
    if(carrito.value[index].cantidad <= 1) return;
    carrito.value[index].cantidad--;
  }

  const incrementarCantidad = (id) =>{
    const index = carrito.value.findIndex(producto => producto.id === id);
    carrito.value[index].cantidad++;
  }

  const eliminarProducto = (id) =>{
    carrito.value = carrito.value.filter(producto => producto.id !== id);
  }

  const vaciarCarito = () =>{
    carrito.value = [];
  }

  const guardarLocalStorage = () =>{
    localStorage.setItem('carrito', JSON.stringify(carrito.value));
  }


</script>

<template>
  
  <!-- Header component -->
  <Header 
    :carritoprops="carrito"
    :guitarraprop="guitarra"
    @decrementar-cantidad="decrementarCantidad"
    @incrementar-cantidad="incrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarito"
  ></Header>

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      
      <Guitarra 
            v-for="guitarra in guitarras" 
            v-bind:guitarraprop="guitarra" 
            @agregar-carrito="agregarCarrito"/>
    </div>

  </main>


  <!-- Footer component -->
  <Footer></Footer>
</template>

<style scoped>
/* css */
</style>
