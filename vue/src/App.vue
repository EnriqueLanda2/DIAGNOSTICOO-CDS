<template>
  
  <ConsumoApi />
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import ConsumoApi from './components/ConsumoApi.vue';
//import MiApp from './components/MiApp.vue';
export default {
  name: 'App',
  components: {
    //HelloWorld,
    //MiApp
    ConsumoApi
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
