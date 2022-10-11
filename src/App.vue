<template>
  <Suspense>
    <template #default>
      <buscarPokemon />
    </template>

    <template #fallback>
      <loading />
    </template>
  </Suspense>
</template>

<script setup>
import { defineAsyncComponent } from "vue";
import loading from "./components/loading.vue";

/*const buscarPokemon = defineAsyncComponent(() =>
  import("./components/buscarPokemon.vue")
);*/

// muestra pantalla de carga
const buscarPokemon = defineAsyncComponent({
  loader: () => import("./components/buscarPokemon.vue"),
  loadingComponent: loading,
  delay: 1000,
  timeout: 3000,
  onError(error, retry, fail, attempts) {
    if (error.message.match(/fetch/) && attempts <= 3) {
      retry();
    } else {
      fail();
    }
  },
  suspensible: true,
});
</script>

<style>

* {
  margin: 0;
  padding: 0;
  list-style: none;
  text-decoration: none;
  border: none;
  outline: none;
  box-sizing: border-box;
}

#app {
  text-align: center;
  font-family: "Poppins", sans-serif;
}
</style>
