<template>
  <div class="repositories-list">
    <input v-model="search" placeholder="Search for a repository" autofocus>

    <div v-for="repository in filteredRepos" class="item">
      <a :href="repository.html_url">
        <h2>{{ repository.full_name }}</h2>
      </a>
    </div>

  </div>
</template>

<script>
  import Github from 'github-api'

  export default {
    name: 'home',
    data () {
      return {
        repositories: [],
        search:       ''
      }
    },

    computed: {
      filteredRepos() {
        return this.filterBy(this.repositories, this.search)
      }
    },

    mounted() {
      let gh = new Github({ token: '4ef964527ceb500a3f7dffd00322e9d7ce977712' })
      gh.getUser().listRepos().then((response) => { this.repositories = response.data })
    }
  }
</script>

<style scoped>
</style>
