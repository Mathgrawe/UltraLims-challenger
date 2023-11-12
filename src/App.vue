<template>
  <router-view></router-view>
</template>

<script>
import desafio1 from "./pages/Desafios/desafio1.vue";
import { RouterView } from "vue-router";

export default {
  components: {
    desafio1,
    RouterView,
  },
};
</script>
