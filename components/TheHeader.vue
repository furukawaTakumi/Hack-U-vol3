<template>
  <div class="header">
      <v-card
        class="headerSpace"
        :class="`d-flex justify-space-between mb-6`"
        color="blue lighten-4"
        :height="100"
        tile>
        <nuxt-link to="">
          <v-img src="https://picsum.photos/510/300?random" width="100" height="100"></v-img>
        </nuxt-link>
        <h1>タイトル(予定地)</h1>
        <v-btn :height="100" :width="100" tile>Menu</v-btn>
      </v-card>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
h1 {
  color: black;
  margin: 25px;
}
.vHeader {
  background-color: white;
}
</style>>
