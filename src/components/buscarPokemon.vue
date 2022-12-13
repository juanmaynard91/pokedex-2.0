<template>
  <header>
    <navbar />
  </header>

  <!-- min-vh-100 -->
  <main class="container">
    <div class="input-group mt-3 mb-3">
      <form @submit.prevent="buscarPokemones">
        <input type="text" class="form-control input" placeholder="ingresa nombre o numero.." v-model.trim="pokemonID" />
      </form>
    </div>

    <transition name="list">
      <!-- Object.entries(pokemones).length > 0 : lo que hace es que solo muestra info cuando tiene algo que mostrar-->
      <div class="centrar" v-if="Object.entries(pokemones).length > 0" ref="target" :style="{ transform: cardTransform }">
        <div class="card mb-3 fw-bold">
          <div class="row g-0">
            <div class="col-md-6">
              <img class="imagen-card" :src="pokemones.sprites.front_default" :alt="pokemones.name" loading="lazy" />
            </div>
            <div class="col-md-6">
              <div class="card-body d-grid gap-3">
                <p>#{{ pokemones.id }}</p>

                <p>{{ pokemones.name }}</p>

                <div class="d-flex justify-content-center align-items-center color-letra">
                  <div class="badge" v-for="(type, index) in pokemones.types" :key="index">
                    <!-- :class si el nombre en css coinside con type.name muestro el color -->
                    <span :class="type.type.name"> {{ type.type.name }}</span>
                  </div>
                </div>

                <div class="color-letra">
                  habilidades :
                  <div v-for="(ability, index) in pokemones.abilities" :key="index">
                    <span>{{ ability.ability.name }}</span>
                  </div>
                </div>

                <div class="color-letra">
                  estadisticas :
                  <div v-for="(stat, index) in pokemones.stats" :key="index">
                    <div>{{ stat.stat.name }} {{ stat.base_stat }}</div>
                  </div>
                </div>

                <!-- los datos del computed estadistica se lo mando por props -->
                <!--<barras :estadisticas="estadisticas" />-->
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </main>
</template>

<script setup>
// instalar con npm para que la app funcione sin internet en vez de usar link o script en html
import barras from "../components/barras.vue";
import navbar from "../components/navbar.vue";
import loading from "../components/loading.vue";
import { ref, computed } from "vue";
import { useMouseInElement } from "@vueuse/core"; // install npm animacion card con el mouse
import Swal from "sweetalert2"; // alert

const target = ref(null);
const { elementX, elementY, isOutside, elementHeight, elementWidth } =
  useMouseInElement(target);
const pokemones = ref([]);
const pokeApi = "https://pokeapi.co/api/v2/pokemon";
const pokemonID = ref("");

// animacion al card cuando paso el mouse
const cardTransform = computed(() => {
  const MAX_ROTATION = 6;

  const rX = ( MAX_ROTATION / 2 - (elementY.value / elementHeight.value) * MAX_ROTATION).toFixed(2);
  const rY = ( (elementX.value / elementWidth.value) * MAX_ROTATION - MAX_ROTATION / 2).toFixed(2);

  return isOutside.value ? "" : `perspective(${elementWidth.value}px) rotateX(${rX}deg) rotateY(${rY}deg)`;});

const cargarLosBichos = async () => {
  try {
    //muestra la imagen hasta el 897
    const res = await fetch(`${pokeApi}/${pokemonID.value}`);
    const data = await res.json();
    pokemones.value = data;
    //console.log(data);
  } catch (error) {
    //alert("la url exploto en mil pedasos rey");
    Swal.fire({
      title: "Error!",
      text: "ingresa bien el nombre del pokemon rey",
      icon: "error",
    });
    console.log(error);
  }
};

const buscarPokemones = () => {
  //event.preventDefault(); //prevengo el comportamiento por defecto
  if (pokemonID.value !== "") {
    //por si el usuario ingresa la primera letra en mayuscula haga la busqueda igual
    pokemonID.value = pokemonID.value.toLocaleLowerCase();
    cargarLosBichos();
    pokemonID.value = "";
  } else {
    pokemones.value = "";
  }
};

//mostrar info del bicho
// muestra un nuevo array por eso le agrega las "", hacer un v-for en template con li
const ataques = computed(() => {
  if (pokemones.value) {
    return pokemones.value.abilities.map((ataques) => ataques.ability.name);
  }
});

const tipo = computed(() => {
  if (pokemones.value) {
    return pokemones.value.types.map((tipo) => tipo.type.name);
  }
});

const estadisticas = computed(() => {
  if (pokemones.value) {
    return pokemones.value.stats.map((estadisticas) => estadisticas.base_stat);
  }
});
</script>

<style>
/* importo los colores de los tipos desde style.css */
@import "../assets/style.css";

.imagen-card {
  height: 25rem;
}

.input {
  margin-bottom: 6rem;
}

@media (max-width: 767px) {
  .imagen-card {
    height: 7rem;
  }
}

@media (max-height: 915px) {
   .input{
    margin-bottom: 6rem;
   }
}

.centrar {
  display: grid;
  place-items: center;
  margin: 0 auto;
}

p {
  margin: 0;
}

span {
  padding: 5px;
}

form {
  width: 100%;
}

/*centro la imagen */
.col-md-4 {
  display: flex;
  justify-content: center;
  align-items: center;
}

.centrar-loading {
  display: flex;
  justify-content: center;
  margin-top: 15rem;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}

.card {
  border-radius: 20px;
}

.card:hover {
  filter: drop-shadow(0 0 15px #121212);
}
</style>
