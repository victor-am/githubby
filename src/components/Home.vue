<template>
  <el-row>
    <el-col :offset="5" :span="14" class="repositories-list">

      <input v-model="search" class="search-box" @keyup.enter="openFirstRepository" placeholder="Search for a repository" autofocus>

      <div class="divider"></div>

      <el-row :gutter="20">
        <el-col v-for="repository in filteredRepos" :key="repository.id" :span="12">
          <el-card class="item">
            <div class="header" slot="header">
              <i class="fa fa-github"></i>

              <a :href="repository.html_url" target="_blank">
                <h3>{{ repository.full_name }}</h3>
              </a>
            </div>

            <p>Notifications: {{ notificationsFrom(repository).length }}</p>
            <p>Last push {{ fromNow(repository.pushed_at) }}</p>
          </el-card>
        </el-col>
      </el-row>

    </el-col>
  </el-row>
</template>

<script>
  import Github from 'github-api'
  import Moment from 'moment'

  export default {
    name: 'home',

    data () {
      return {
        repositories:  [],
        notifications: [],
        search:        ''
      }
    },

    computed: {
      sortedRepos() {
        return this.orderBy(this.repositories, 'pushed_at', -1)
      },

      filteredRepos() {
        return this.filterBy(this.sortedRepos, this.search)
      }
    },

    methods: {
      openFirstRepository() {
        let repository = this.filteredRepos[0]
        window.open(repository.html_url)
        this.search = ''
      },

      fromNow(time) {
        return Moment(time).fromNow()
      },

      notificationsFrom(repository) {
        return this.notifications.filter((n) => {
          return n.repository.id === repository.id
        })
      }
    },

    mounted() {
      let token = localStorage.getItem('github_token')

      if (token) {
        let gh   = new Github({ token })
        let user = gh.getUser()

        user.listRepos().then((response) => {
          this.repositories = response.data
        }).then(() => {
          return user.listNotifications()
        }).then((response) => {
          this.notifications = response.data
        })
      }
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

  .repositories-list .item {
    margin: 1rem 0;

    .header {
      width:         100%;
      white-space:   nowrap;
      overflow:      hidden;
      text-overflow: ellipsis;

      i {
        font-size: 1.5rem;
        margin-right: 1rem;
          float: left;
      }

      h3 {
        font-size: 1.1rem;
        margin:    0;
      }
    }

    p {
      margin: 0.5rem 0;
    }
  }
</style>
