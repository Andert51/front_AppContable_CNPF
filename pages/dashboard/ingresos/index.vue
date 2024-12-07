<template>
  <div>
    <h1>Ingresos</h1>
    <saleInsight />
  </div>
</template>

<script>
import saleInsight from '~/components/ui/saleInsight.vue'
export default {
  components: {
    saleInsight
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
