<template>
  <v-card
    :class="`d-flex justify-space-between mb-6`"
    color="blue lighten-4"
    :height="100"
    tile
  >
    <nuxt-link to="">
      <v-img src="Logo.png" width="100" height="120" />
    </nuxt-link>
    <a>Speed Mania</a>
    <v-btn :height="0" :width="0" tile />
  </v-card>
</template>

<script>
export default {}
</script>

<style scoped>
a {
  white-space: nowrap;
  font-family: 'Impact';
  font-size: 60px;
}
</style>
