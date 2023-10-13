<template>
  <TapInGenerator title="Tap-in Generator"/>
</template>

<script>
// Components
import TapInGenerator from '@/components/TapInGenerator.vue'

export default {
  name: 'App',
  components: { TapInGenerator }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
}

html {
  background: #2c3e50;
}
</style>
