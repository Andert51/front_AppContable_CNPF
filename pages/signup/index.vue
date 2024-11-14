<template>
  <signup-form />
</template>

<script>
import signupForm from '~/components/ui/signupForm.vue'
export default {
  components: {
    signupForm
  },
  auth: false,
  layout: 'signup'
}
</script>

<style scoped>

</style>
