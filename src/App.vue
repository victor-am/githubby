<template>
  <div id="app">
    <h1>
      <i class="fa fa-github-alt"></i>
      Githubby
    </h1>

    <input v-model="githubToken"/> <button @click="saveToken">Save</button>

    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app',

  data() {
    return {
      githubToken: localStorage.getItem('github_token')
    }
  },

  methods: {
    saveToken() {
      localStorage.setItem('github_token', this.githubToken)
    }
  }
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
