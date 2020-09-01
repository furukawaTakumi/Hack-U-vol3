<template>
  <v-app class="vApp">
    <nuxt />
    <the-footer />
  </v-app>
</template>

<script>
import TheFooter from '@/components/TheFooter'

export default {
  components: {
    TheFooter
  }
}
</script>

<style scoped>
</style>
