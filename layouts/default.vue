<template>
  <div>
    <v-app>
      <TheHeader />
    </v-app>
      <nuxt />
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
