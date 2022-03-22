<template>
  <div id="app">
    <Plusses />
  </div>
</template>

<script>
import Plusses from './components/Plusses.vue'
export default {
  name: 'App',
  components: {
    Plusses
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
