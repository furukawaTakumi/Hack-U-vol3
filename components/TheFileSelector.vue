<template>
  <div class="filePikcer">
      <v-file-input accept="image/*" label="File input"></v-file-input>
    <a>File Size:{{ fileSize }}</a>
  </div>
</template>

<script>
export default {
  data: () => ({
    fileSize: 0
  }),
  methods: {
    GetFileSize () {

    }
  }
}

</script>

<style scoped>
</style>
