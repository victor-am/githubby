<template>
  <div id="app">
    <h1>
      <i class="fa fa-github-alt"></i>
      Githubby
    </h1>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing:  antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color:      #2c3e50;
  margin-top: 2rem;
}
</style>
