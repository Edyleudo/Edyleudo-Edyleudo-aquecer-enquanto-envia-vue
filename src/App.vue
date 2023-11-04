<template>
  <img style="height: 100px;" alt="Vue logo" src="./assets/logo.png">
  <ListJson msg="Welcome to Your Vue.js App"/>
</template>

<script>
import ListJson from './components/ListJson.vue';

export default {
  name: 'App',
  components: {
    ListJson,
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
