<template>
  <el-card class="repository">
    <div class="header" slot="header">

      <a :href="repository.notifications_url" target="_blank">
        <el-badge :value="notifications.length" :max="99">
          <i class="fa fa-github"/>
        </el-badge>
      </a>

      <a :href="repository.html_url" target="_blank">
        <h3>{{ repository.full_name }}</h3>
      </a>
    </div>

    <p>Last push {{ lastPush }}</p>
  </el-card>
</template>

<script>
  import Moment from 'moment'

  export default {
    name: 'repository',

    props: {
      repository:    { type: Object, required: true },
      notifications: { type: Array, required: true }
    },

    computed: {
      lastPush() {
        return Moment(this.repository.last_push).fromNow()
      }
    }
  }
</script>

<style lang="scss" scoped>
  a {
    color:           inherit;
    text-decoration: none;
    cursor:          pointer;
  }

  .repository {
    margin: 1rem 1rem;

    .header {
      width:         100%;

      el-badge {
        margin-right: 1rem;
        float:        left;
      }

      i {
        font-size:    1.5rem;
      }

      h3 {
        font-size: 1.1rem;
        margin:    0;
      white-space:   nowrap;
      overflow:      hidden;
      text-overflow: ellipsis;
      }
    }

    p {
      margin: 0.5rem 0;
    }
  }
</style>
