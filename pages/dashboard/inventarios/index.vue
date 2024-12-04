<template>
  <div>
    <h1>Inventarios</h1>
    <productsCrud />
  </div>
</template>

<script>
import productsCrud from '~/components/ui/productsCrud.vue'
export default {
  components: {
    productsCrud
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
