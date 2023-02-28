<template>
  <div id="app">
    <MainApp :UI_Width="50"/>
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

  display: flex;
  justify-content: center;    
  align-items: center; 
}
</style>
