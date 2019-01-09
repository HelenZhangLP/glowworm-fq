<template>
  <div class="dashboard-container">
    <component :is="currentRole"/>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import adminDashboard from './admin'
import editorDashboard from './editor'
import commonDashboard from './common'

export default {
  name: 'Dashboard',
  components: { adminDashboard, editorDashboard, commonDashboard },
  data() {
    return {
      currentRole: 'adminDashboard'
    }
  },
  computed: {
    ...mapGetters([
      'roles'
    ])
  },
  created() {
    this.currentRole = 'editorDashboard'
  }
}
</script>
