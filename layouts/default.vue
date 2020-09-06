<template>
  <v-app>
    <the-header />
    <Test />
    <nuxt />
    <the-footer />
  </v-app>
</template>
<script>
import TheHeader from '@/components/TheHeader'
import TheFooter from '@/components/TheFooter'
import Test from '@/components/SpeedTest'

export default {
  components: {
    TheHeader,
    TheFooter,
    Test
  }
}
</script>
