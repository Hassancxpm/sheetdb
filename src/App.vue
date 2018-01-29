<template>
  <div id="app">
    <img src="./assets/logo.png">
    <SheetDb/>
  </div>
</template>

<script>
import SheetDb from './components/SheetDb'

export default {
  name: 'App',
  components: {
    SheetDb
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
