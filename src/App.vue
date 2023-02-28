<template>
  <div id="app">
    <MainApp/>
  </div>
</template>

<script>
import MainApp from './components/MainApp.vue'

export default {
  name: 'App',
  components: {
    MainApp
  }
}
</script>
<style>
body{
  margin: 0;
  padding: 0;
}
#app{
  background-color: #fafafa;
  height: 100vh;
  width: 100vw;
}
</style>
