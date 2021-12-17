<template>
  <div>
    <Tb />
  </div>
</template>

<script>
import Tb from './components/Table.vue'

export default {
  name: 'App',
  components: {
    Tb
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
