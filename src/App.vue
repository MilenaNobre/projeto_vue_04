<template>
  <div id="app">
      <h1> Calculadora </h1>
      <Calculator></Calculator>

  </div>
</template>

<script>
import Calculator from "@/main/Calculator";
export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>

<style>
#app {
    display: flex;
    flex-direction: column;
    height: 100vh;
    justify-content: center;
    align-items: center;
    text-align: center;
    color: #fff;
}
</style>
