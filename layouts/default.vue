<template>
  <v-app>
    <the-header />
    <nuxt />
    <the-hooter />
  </v-app>
</template>
<script>
import TheHeader from '@/components/TheHeader'
import TheFooter from '@/components/TheFooter'

export default {
  components: {
    TheHeader,
    TheFooter
  }
}
</script>
