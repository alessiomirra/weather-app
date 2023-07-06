<template>
  <MainView /> 
  <p class="text-center text-muted">Alessio Mirra, 2023</p>
</template>

<script>
import MainView from '@/components/View.vue'
export default {
  name: 'App',
  components: {
    MainView, 
  }, 
  created(){
    document.title = "Weather"; 
  }
}
</script>

<style>
body{
  background: linear-gradient(to bottom, #8ecae6, #ffffff);
}
</style>
