<template>
  <v-app class="vApp">
    <the-file-selector class="mainFilePicker" />
    <nuxt />
    <the-footer />
  </v-app>
</template>

<script>
import TheFooter from '@/components/TheFooter'
import TheFileSelector from '@/components/TheFileSelector'

export default {
  components: {
    TheFooter,
    TheFileSelector
  }
}
</script>

<style scoped>
.mainFilePicker {
  width: 600px;
  height: 600px;
}
</style>
