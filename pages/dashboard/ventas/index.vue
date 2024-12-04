<template>
  <div>
    <h1>Ventas</h1>
    <transaction-crud />
  </div>
</template>

<script>
import transactionCrud from '~/components/ui/transactionCrud.vue'
export default {
  components: {
    transactionCrud
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
