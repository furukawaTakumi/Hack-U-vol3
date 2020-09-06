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
          v-if="index < showFileChipCount"
          color="deep-purple accent-4"
          dark
          label
          small
        >
          {{ text }}
        </v-chip>

        <span
          v-else-if="index === showFileChipCount"
          class="overline grey--text text--darken-3 mx-2"
          >+{{ files.length - showFileChipCount }} File(s)</span
        >
      </template>
    </v-file-input>
  </div>
</template>

<script>
export default {
  props: {
    showFileChipCount: {
      type: Number,
      required: true,
    },
  },
  data: () => ({
    fileSize: 0,
    files: [],
    inputHeight: 0,
  }),
  mounted() {
    this.setPickerHeight()
  },
  methods: {
    setFileSize(e) {
      for (let i = 0; i < e.length; i++) {
        this.fileSize += e[i].size
      }
      this.$emit('fileSelected')
    },
    getFileSize() {
      return this.fileSize
    },
    setPickerHeight() {
      const filePicker = document.getElementById('filePicker')
      const filePickerStyle = getComputedStyle(filePicker)
      const height = filePickerStyle.height.match(/(?<hValue>[0-9]+).*/).groups
        .hValue
      this.inputHeight = height - 30
    },
  },
}
</script>

<style scoped>
.filePikcer {
  width: 100%;
  height: 100%;
}
</style>
