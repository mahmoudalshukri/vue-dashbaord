<template>
  <div id="app">
    <PriorityDashboard />
  </div>
</template>

<script>
import PriorityDashboard from './components/PriorityDashboard.vue'

export default {
  name: 'App',
  components: {
    PriorityDashboard
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}
</style>