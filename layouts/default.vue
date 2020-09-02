<template>
  <div>
    <v-app>
      <the-header />
      <nuxt />
    </v-app>
  </div>
</template>

<script>
import TheHeader from '@/components/TheHeader'

export default {
  components: {
    TheHeader
  }
}
</script>
