<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <SimonTheGame />
  </div>
</template>

<script>
import SimonTheGame from "@/components/SimonTheGame";

export default {
  name: 'App',
  components: {
    SimonTheGame
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
