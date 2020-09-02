<template>
  <v-footer
    dark
    padless
  >
    <v-card
      class="flex"
      flat
      tile
    >
      <v-card-title class="teal">
        <strong class="subheading">You're going to be the Internet!</strong>
        <v-spacer />
        <v-btn href="https://github.com/furukawaTakumi/Hack-U-vol3" target="_blank">
          <v-icon>mdi-github</v-icon>
        </v-btn>
        <v-btn href="https://twitter.com/set_official" target="_blank">
          <v-icon>mdi-twitter</v-icon>
        </v-btn>
        <v-btn href="https://set1.ie.aitech.ac.jp/" target="_blank">
          <v-icon>mdi-google-chrome</v-icon>
        </v-btn>
      </v-card-title>
      <v-card-text class="py-2 white--text text-center">
        {{ new Date().getFullYear() }} — <strong>しすけんそるじゃーず</strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  data: () => ({
    icons: [
      'mdi-twitter',
      'mdi-github',
      'mdi-apple-safari'
    ]
  })
}

</script>

<style scoped>
</style>
