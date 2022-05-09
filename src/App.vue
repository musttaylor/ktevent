<!DOCTYPE html>
<template>
  <div bgcolor="white" id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
/*
#app {
  color-scheme: only light;
  background-color: white;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px 0px 0px 0px;
  padding: 0px;
}
*/

@media (prefers-color-scheme: light) {
  #app {
    color-scheme: only light;
    background-color: white;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin: 0px 0px 0px 0px;
    padding: 0px;
  }
}

@media (prefers-color-scheme: dark) {
  #app {
    color-scheme: only light;
    background-color: white;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin: 0px 0px 0px 0px;
    padding: 0px;
  }
}

body {
  margin: 0;
  padding: 0;
}
</style>
