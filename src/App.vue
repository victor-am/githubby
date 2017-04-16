<template>
  <div id="app">
    <div class="avatar">
      <img :src="user.avatar_url"/>
      <div class="hover-dim">
        <i class="fa fa-cog"/>
      </div>
    </div>

    <h1>
      <i class="fa fa-github-alt"></i>
      Githubby
    </h1>

    <input v-model="githubToken"/> <button @click="saveToken">Save</button>

    <router-view></router-view>
  </div>
</template>

<script>
  import Github     from 'github-api'

  export default {
    name: 'app',

    data() {
      return {
        user: {},
        githubToken: localStorage.getItem('github_token')
      }
    },

    methods: {
      saveToken() {
        localStorage.setItem('github_token', this.githubToken)
      }
    },

    mounted() {
      let token = localStorage.getItem('github_token')

      if (token) {
        let gh   = new Github({ token })

        gh.getUser().getProfile().then((response) => {
          this.user = response.data
        })
      }
    }
  }
</script>

<style lang="scss">
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing:  antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color:      #2c3e50;
    margin-top: 2rem;
  }

  .avatar {
    width:         50px;
    height:        50px;
    overflow:      hidden;
    border:        3px solid #c5c5c5;
    border-radius: 100rem;
    position:      absolute;
    right:         10px;
    top:           10px;
    cursor:        pointer;

    img {
      width: 100%;
    }

    .hover-dim {
      position:   absolute;
      top:        0;
      left:       0;
      background: rgba(10, 10, 10, 0.4);
      width:      100%;
      height:     100%;
      opacity:    0;
      -webkit-transition: opacity 0.3s; /* Safari */
      transition:         opacity 0.3s;

      &:hover {
        opacity: 1;
      }

      i {
        color:      #ddd;
        margin-top: 9px;
        font-size:  2rem;
        opacity:    0.9;
      }
    }
  }

</style>
