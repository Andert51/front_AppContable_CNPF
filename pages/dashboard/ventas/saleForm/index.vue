<template>
  <div>
    <h1>Nueva Factura</h1>
    <saleForm />
  </div>
</template>

<script>
import saleForm from '~/components/ui/saleForm.vue'
export default {
  components: {
    saleForm
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
