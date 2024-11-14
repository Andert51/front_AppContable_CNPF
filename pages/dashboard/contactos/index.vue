<template>
  <div>
    <h1>Clientes</h1>
    <clientsCrud />
  </div>
</template>

<script>
import clientsCrud from '~/components/ui/clientsCrud.vue'
export default {
  components: {
    clientsCrud
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
