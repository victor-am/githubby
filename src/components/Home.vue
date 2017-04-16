<template>
  <el-row>
    <el-col :offset="5" :span="14" class="repositories-list">

      <el-input v-model="search" placeholder="Search for a repository" autofocus>
        <el-button slot="append" icon="search"></el-button>
      </el-input>

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

            Last push {{ fromNow(repository.pushed_at) }}
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
        repositories: [],
        search:       ''
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
      fromNow(time) {
        return Moment(time).fromNow()
      }
    },

    mounted() {
      let token = localStorage.getItem('github_token')

      if (token) {
        let gh = new Github({ token })
        gh.getUser().listRepos().then((response) => { this.repositories = response.data })
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
  }
</style>
