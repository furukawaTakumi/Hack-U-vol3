<template>
  <div class="filePikcer">
    <v-file-input
      height="320px"
      v-model="files"
      color="deep-purple accent-4"
      counter
      label="送信ファイル"
      prepend-icon
      multiple
      outlined
      :show-size="1000"
      @change="setFileSize"
    >
      <template v-slot:selection="{ index, text }">
        <v-chip v-if="index < 8" color="deep-purple accent-4" dark label small>{{ text }}</v-chip>

        <span
          v-else-if="index === 8"
          class="overline grey--text text--darken-3 mx-2"
        >+{{ files.length - 8 }} File(s)</span>
      </template>
    </v-file-input>
  </div>
</template>

<script>
export default {
  data: () => ({
    fileSize: 0
  }),
  methods: {
    setFileSize (e) {
      for (let i = 0; i < e.length; i++) {
        this.fileSize += e[i].size
      }
    },
    getFileSize () {
      return this.fileSize
    }
  }
}
</script>

<style scoped>
.filePikcer {
  width: 320px;
  height: 320px;
}
</style>
