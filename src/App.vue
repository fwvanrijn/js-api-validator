<template>
    <img alt="Vue logo" src="./assets/logo.png">
    <api-validator></api-validator>
</template>

<script>
import ApiValidator from "@/components/ApiValidator";

export default {
  name: 'App',
  components: {
    ApiValidator
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
