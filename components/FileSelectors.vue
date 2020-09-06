<template>
  <div id="filePicker" class="filePikcer">
    <v-file-input
      v-model="files"
      color="deep-purple accent-4"
      counter
      label="送信ファイル"
      prepend-icon
      multiple
      outlined
      :height="inputHeight"
      @change="setFileSize"
    >
      <template v-slot:selection="{ index, text }">
        <v-chip
          v-if="index < 8"
          color="deep-purple accent-4"
          dark
          label
          small
        >
          {{ text }}
        </v-chip>

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
    fileSize: 0,
    files: [],
    inputHeight: 0
  }),
  mounted () {
    this.setPickerHeight()
  },
  methods: {
    setFileSize (e) {
      for (let i = 0; i < e.length; i++) {
        this.fileSize += e[i].size
      }
    },
    getFileSize () {
      return this.fileSize
    },
    setPickerHeight () {
      const element = document.getElementById('filePicker')
      const cssStyleDeclaration = getComputedStyle(element, null)
      const height = cssStyleDeclaration.getPropertyValue('height')
      this.inputHeight = height
    }
  }
}
</script>

<style scoped>
.filePikcer {
  width: 100%;
  height: 100%;
}
</style>
