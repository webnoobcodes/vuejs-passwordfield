<template>
  <div id="app">
    <PasswordField />
  </div>
</template>

<script>
  import PasswordField from './components/PasswordField.vue'

  export default {
    name: 'app',
    components: {
      PasswordField
    }
  }
</script>

<style lang="scss">
  body {
    margin: 0;
    padding: 0;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: linear-gradient(to bottom left, #04AFA8, #2F3A45);
  }

  #app {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 1.8rem;
    color: #333;
  }
</style>
