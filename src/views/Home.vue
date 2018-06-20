<template>
  <div class="home">
    <ProductsList />
  </div>
</template>

<script>
  import ProductsList from '@/components/ProductsList.vue'

  export default {
    name: 'home',
    components: {
      ProductsList
    }
  }
</script>
