<template>
  <HomeComponent/>
</template>

<script>
import HomeComponent from './components/HomeComponent.vue'

export default {
  name: 'App',
  components: {
    HomeComponent
  }
}
</script>

<style>
body{
  background-color: #121212 !important;
}
.header{
  background-color: #212730;
  border-radius: 20px;
  color: white;
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  margin-top: 2rem;
}
.btn-search{
  background-image: linear-gradient(to right, cyan, magenta);
}
</style>
