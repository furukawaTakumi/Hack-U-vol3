<template>
  <div class="file-size-input">
    <div>
      <v-input v-model.number="fileSizeNum" type="number" />
    </div>
    <div></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fileSizeNum: 0,
    }
  },
}
</script>

<style lang="scss" scoped>
.file-size-input {
  display: flex;
}
</style>
