<template>
  <div class="header">
    <v-app id="inspire" class="vHeader">
      <v-card
        class="headerSpace"
        :class="`d-flex justify-space-between mb-6`"
        color="blue lighten-4"
        :height="100"
        >
        <nuxt-link to="">
        <v-img src="https://picsum.photos/510/300?random" width="100" height="100"></v-img>
        </nuxt-link>
          <div id="chatchPhrase">
            <h1>タイトル(予定地)</h1>
          </div>
          <v-btn :height="100" :width="100" tile>Menu</v-btn>
        </v-card>
    </v-app>
  </div>
</template>

<script>
export default {
  methods: {
    AcsTop: {

    }
  }
}
</script>

<style scoped>
h1 {
  color: black;
  margin: 25px;
}
.v_header {
  background-color: white;
}
</style>>
