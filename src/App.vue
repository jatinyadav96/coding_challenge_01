<template>
    <div id="app">
    <DrawingFiles />
  </div>
</template>

<script>
import DrawingFiles from './components/DrawingFiles.vue';
// import pdf from 'vue-pdf';
export default {
  name: 'App',
  components: {
    DrawingFiles,
    // pdf,
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
