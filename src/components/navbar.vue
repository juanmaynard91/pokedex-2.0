<template>
  <nav class="navbar navbar-dark bg-dark">
    <div class="container-fluid acomodar-navbar">
      <a class="navbar-brand">
        <img src="../assets/logo-pokemon.png" alt="logo pokemon" loading="lazy" />
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

img {
  height: 3rem;
}
</style>
