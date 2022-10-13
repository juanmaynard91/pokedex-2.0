<template>
  <nav class="navbar navbar-dark bg-dark">
    <div class="container-fluid acomodar-navbar">
      <a class="navbar-brand">
        <h3>Busca tu pokemon pa</h3>
      </a>
      <cambiarTema />
    </div>
  </nav>
</template>

<script setup>
import cambiarTema from "./cambiarColor.vue";
</script>

<style scoped>
a { 
  margin: 0;
}
</style>
