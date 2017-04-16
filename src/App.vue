<template>
  <div id="app">
    <el-menu theme="dark" mode="horizontal">
      <el-menu-item index="1">
        <div class="avatar">
          <img :src="user.avatar_url"/>
          <div class="hover-dim">
            <i class="fa fa-cog"/>
          </div>
        </div>
      </el-menu-item>
    </el-menu>

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
    position:    absolute;
    top:         0;
    left:        0;
    width:       100%;
    text-align:  center;
    color:       #2c3e50;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing:  antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  h1 {
    margin-top: 3rem;
  }

  .avatar {
    width:         40px;
    height:        40px;
    overflow:      hidden;
    border:        2px solid #c5c5c5;
    border-radius: 100rem;
    position:      relative;
    cursor:        pointer;
    margin-top:    7px;

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
        position:    absolute;
        left:        50%;
        top:         50%;
        margin-top:  -10px;
        margin-left: -10px;
        color:       #ddd;
        font-size:   1.4rem;
        opacity:     0.9;
      }
    }
  }

</style>
