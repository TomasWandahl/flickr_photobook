<template>
  <div id="app">
    <PhotoBook />
  </div>
</template>

<script>
import PhotoBook from "./components/PhotoBook"

export default {
  name: 'App',
  components: {
    PhotoBook
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background-color: #286166;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
