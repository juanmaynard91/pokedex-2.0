<template>
  <div class="container altura-barras">
    <apexchart width="350" type="bar" :options="options" :series="series"></apexchart>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  estadisticas: Array,
});

const options = {
  chart: {
    id: "vuechart-example",
  },
  xaxis: {
    categories: ["Vida", "Ataque", "Defensa", "Ataque Esp.", "Defensa Esp.", "Velocidad" ],
  },
};

const series = computed(() => {
  return [
    {
      name: "estadisticas",
      data: props.estadisticas,
    },
  ];
});

//asi viene en la pagina de barras
/*const series = [
  {
    name: "series-1",
    data: [30, 40, 45, 50, 49, 60, 70, 91],
  },
];*/
</script>

<style>
/*quito margen del lado derecho de las barras con margin auto*/
#apexchartsvuechart-example {
  margin: auto;
}

.altura-barras{
  height: 11.3rem;
}
</style>