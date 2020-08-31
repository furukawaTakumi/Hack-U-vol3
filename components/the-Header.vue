<template>
  <div class="header">
    <v-app id="inspire">
      <div>
        <v-card class="header_space" :key="j" :class="`d-flex justify-space-between mb-6`" color="blue lighten-4" :height="100">
          <v-card :key="s" img="https://picsum.photos/510/300?random" :width="100" class="logo" tile>
          </v-card>
          <div id="chatchPhrase">
            <h1>タイトル(予定地)</h1>
          </div>
          <v-card :key="t" :width="100" class="menu" outlined tile>
            <v-btn :height="100" :width="100">Menu</v-btn>
          </v-card>
        </v-card>
      </div>
    </v-app>
    <div class="header_space"></div>
    <div class="menu"></div>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
/* .logo {
  background-color:teal;
} */
.menu {
  background-color:teal;
}
h1 {
  color:black;
  margin: 25px;
}
.header_space {
  background-color:rgb(36, 28, 146);
}
/* .title_logo {
    height: 150px;
    width: 150;
    background-color:teal;

    margin-right: -3px;
  }
  .header_space {
    margin-left: -3px;
    margin-right: -50px;

    display: inline-block;
    height: 150px;
    width: 80%;
    background-color: rgb(69, 212, 212);
  }
  .menu {
    float: right;
    height: 150px;
    width: 150px;

    margin-left: -1px;

    background-color: brown;
  } */
</style>>
