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
      :show-size="1000"
      :height="inputHeight"
      @change="setFileSize"
    >
      <template v-slot:selection="{ index, text }">
        <v-chip v-if="index < 8" color="deep-purple accent-4" dark label small>
          {{ text }}
        </v-chip>

        <span
          v-else-if="index === 8"
          class="overline grey--text text--darken-3 mx-2"
        >+{{ files.length - 8 }} File(s)</span>
      </template>
    </v-file-input>
    <button @click="setPickerHeight">
      aaaaa
    </button>
  </div>
</template>

<script>
export default {
  data: () => ({
    fileSize: 0,
    files: [],
    inputHeight: 200
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
      // this.inputHeight = document.getElementById('filePicker').style.height
      window.onload(() => {
        alert(document.getElementById('filePicker').style.height)
      })
    }
  }
}
</script>

<style scoped>
.filePikcer {
  background-color: aqua;
  width: 100%;
  height: 100%;
}
</style>
