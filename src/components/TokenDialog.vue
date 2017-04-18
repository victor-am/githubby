<template>
  <el-dialog title="Setup" v-model="showDialog">
    <h3>1. Generate your token with access to <strong>User and Repositories</strong></h3>
    <el-button @click="openGenerateTokenPage">
      Generate Token
    </el-button>

    <h3>2. Copy and paste your token here</h3>
    <el-form>
      <el-form-item>
        <el-input v-model="githubToken" auto-complete="off" placeholder="Insert your Github token"/>
      </el-form-item>
    </el-form>

    Your token will be kept only in your browser.

    <span slot="footer" class="dialog-footer">
      <el-button @click="$emit('close')">Close</el-button>
      <el-button type="primary" @click="saveToken">Save</el-button>
    </span>
  </el-dialog>
</template>

<script>
  import Github from 'github-api'

  export default {
    name: 'app',

    data() {
      return { githubToken: '' }
    },

    props: {
      showDialog: { required: true, default: false }
    },

    methods: {
      saveToken() {
        localStorage.setItem('github_token', this.githubToken)
        this.$emit('close')
      },

      openGenerateTokenPage() {
        window.open('https://github.com/settings/tokens/new')
      }
    }
  }
</script>

<style lang="scss">
</style>
