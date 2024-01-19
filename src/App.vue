<script setup>
import { ref } from "vue";
import VueOpenMap from './components/VueOpenMap.vue'
const objMap = ref({});
const mapChange =  (arg) => {
  objMap.value = arg;
  console.log(objMap.value)
}
</script>

<template>
  <div style="width: 70%; position: relative; height: 600px;">
    <VueOpenMap @on-change="mapChange" :adaptive="true" />
  </div>
  <div style="background: white; position: absolute; padding: 20px; bottom: 0; width: 100%; left: 0; z-index: 1000;">
    {{ objMap.address_name }}
    ({{ objMap.coordinates?.lat }}, {{ objMap.coordinates?.lon }})
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
