<template>
  <div id="app">
    <base-image-input/>
  </div>
</template>

<script>
import BaseImageInput from './components/BaseImageInput'

export default {
  name: 'App',
  components: {
     BaseImageInput 
  },
  methods: {
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
