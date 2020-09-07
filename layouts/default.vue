<template>
  <v-app>
    <the-header />
    <speedTest />
    <nuxt />
    <the-footer />
  </v-app>
</template>
<script>
import TheHeader from '@/components/TheHeader'
import TheFooter from '@/components/TheFooter'
import SpeedTest from '@/components/SpeedTest'

export default {
  components: {
    TheHeader,
    TheFooter,
    SpeedTest
  }
}
</script>
