<template>
  <el-row>
    <el-col :offset="5" :span="14" class="repositories-list">

      <input v-model="search"
             class="search-box"
             @keyup.enter="openFirstRepository"
             placeholder="Search for a repository"
             autofocus/>

      <div class="divider"></div>

      <el-row>
        <el-col v-for="repository in filteredRepos" :span="12" :key="repository.id">
          <repository :repository="repository"
                      :notifications="notificationsFrom(repository)"/>
        </el-col>
      </el-row>

    </el-col>
  </el-row>
</template>

<script>
  import Github     from 'github-api'
  import Repository from '@/components/Repository'

  export default {
    name: 'home',

    components: { Repository },

    data () {
      return {
        user:          {},
        repositories:  [],
        notifications: [],
        search:        ''
      }
    },

    props: {
      githubToken: { type: String, required: true }
    },

    computed: {
      sortedRepos() {
        return this.orderBy(this.repositories, 'pushed_at', -1)
      },

      filteredRepos() {
        return this.filterBy(this.sortedRepos, this.search)
      }
    },

    watch: {
      githubToken(newValue) {
        this.githubToken = newValue
        if (this.githubToken) { this.fetchData() }
      }
    },

    methods: {
      openFirstRepository() {
        let repository = this.filteredRepos[0]

        if (repository) {
          window.open(repository.html_url)
          this.search = ''
        }
      },

      notificationsFrom(repository) {
        return this.notifications.filter((n) => {
          return n.repository.id === repository.id
        })
      },

      fetchData() {
        let gh   = new Github({ token: this.githubToken })
        let user = gh.getUser()

        user.listRepos().then((response) => {
          this.repositories = response.data
          return user.listNotifications()
        }).then((response) => {
          this.notifications = response.data
        })
      }
    },

    mounted() {
      if (this.githubToken) { this.fetchData() }
    }
  }
</script>

<style lang="scss" scoped>
  .divider {
    margin:           1.5rem 0 0.5rem 0;
    width:            100%;
    height:           1px;
    background-color: #eaeaea;
  }

  .repositories-list .search-box {
    width: calc(100% - 1.6rem - 2px);
    padding: 0.7rem 0.8rem;
    font-size: 0.8rem;
    border-radius: 5px;
    border: 1px solid #c0c9ce;

    &:focus {
      outline: none;
      border: 1px solid #20a0ff;
    }
  }
</style>
