<template>
  <div>
    <sidebar-dash-vue />
  </div>
</template>

<script>
import sidebarDashVue from '~/components/ui/sidebarDash.vue'
export default {
  components: {
    sidebarDashVue
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
