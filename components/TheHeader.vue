<template>
  <v-card
    :class="`d-flex justify-space-between mb-6`"
    color="blue lighten-4"
    :height="100"
    tile
  >
    <nuxt-link to="">
      <v-img
        src="https://picsum.photos/510/300?random"
        width="100"
        height="100"
      />
    </nuxt-link>
    <h1>タイトル(予定地)</h1>
    <v-btn :height="100" :width="100" tile> Menu </v-btn>
  </v-card>
</template>

<script>
export default {}
</script>

<style scoped>
h1 {
  white-space: nowrap;
  color: black;
  margin: 25px;
}
</style>
>
