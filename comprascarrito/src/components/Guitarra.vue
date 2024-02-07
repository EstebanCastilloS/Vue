<script setup>


  const props = defineProps({
    guitarraprop: {
      type: Object,
      required: true
    }
  })

  defineEmits(['agregar-carrito'])

  // const incrementar = () => {
  //   numero.value++
  // }
</script>


<template>
  <div class="col-md-6 col-lg-4 my-4 row align-items-center">
    <div class="col-4">
      <img class="img-fluid" v-bind:src="'/img/' + guitarraprop.imagen + '.jpg'"
        :alt="'imagen guitarra-' + guitarraprop.nombre">
    </div>
    <div class="col-8">
      <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitarraprop.nombre }}</h3>
      <p>{{ guitarraprop.descripcion }}</p>
      <p class="fw-black text-primary fs-3">$ {{ guitarraprop.precio }}</p>
      <button 
          type="button" 
          class="btn btn-dark w-100 " 
          @click="$emit('agregar-carrito', guitarraprop)"
          
          >Agregar al Carrito</button>
    </div>
  </div>
  <!-- FIN GUITARRA -->
</template>



<style></style>