
<style>
  @font-face {
    font-family: 'Calculator';
    src: url("/Calculator.ttf") format("TrueType")
  }

  body {
    margin: 0;
  }

  * {
    box-sizing: border-box
  }

  #app {
    display: grid;
    place-items: center;
    height: 100vh;
  }

  :any-link {
    color: inherit;
    text-decoration: none;
  }
</style>

<template>
  <div id="app">
    <Calculator msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>
