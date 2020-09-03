<template>
  <v-app>
    <the-header />
    <MapGeocoding />
    <nuxt />
    <the-footer />
  </v-app>
</template>
<script>
import TheHeader from '@/components/TheHeader'
import TheFooter from '@/components/TheFooter'
import MapGeocoding from '@/components/MapGeocoding'

export default {
  components: {
    TheHeader,
    TheFooter,
    MapGeocoding
  }
}
</script>
