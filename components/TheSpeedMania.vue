<template>
  <div>
    <v-container>
      <v-row v-show="nowState === stateItem[0]" class="section">
        <div class="file-size-input-section">
          <file-size-inputs
            v-if="inputFileFaild"
            ref="fileSizeInput"
            @input="checkFileSizeInput"
          />
          <file-selectors v-else :showFileChipCount="5" class="file-selector" />
        </div>
      </v-row>
      <v-row v-show="nowState === stateItem[1]" class="section">
        <div class="travel-time-calc-section">
          <travel-time-calculators />
        </div>
      </v-row>
      <v-row v-show="nowState === stateItem[2]" class="section">
        <div class="speed-test-section">
          <speed-test />
        </div>
      </v-row>
      <v-row v-show="nowState === stateItem[3]"></v-row>
    </v-container>
    <div id="navigation-btn">
      <state-full-buttons @click="proceedInput" ref="stateFullBtn" />
    </div>
  </div>
</template>

<script>
import TravelTimeCalculators from '@/components/TravelTimeCalculators'
import FileSizeInputs from '@/components/FileSizeInputs'
import FileSelectors from '@/components/FileSelectors'
import SpeedTest from '@/components/SpeedTest'
import StateFullButtons from '@/components/StateFullButtons'

export default {
  components: {
    TravelTimeCalculators,
    FileSizeInputs,
    FileSelectors,
    SpeedTest,
    StateFullButtons,
  },
  mounted() {
    this.$refs.stateFullBtn.changeRejectState()
  },
  data() {
    return {
      nowState: 'no-input',
      inputFileFaild: true,
      stateItem: [
        'no-input',
        'confirmed file size',
        'confirmed travel time ',
        'finished speed test ',
      ],
      inputsData: {
        fileSize: null,
      },
    }
  },
  methods: {
    checkFileSizeInput(fileSize) {
      if (fileSize === 0) {
        this.$refs.stateFullBtn.changeRejectState()
        this.fileSize = null
      } else {
        this.$refs.stateFullBtn.changeAcceptState()
        this.fileSize = fileSize
      }
    },
    proceedInput(btnState) {
      if (!btnState) {
        alert('入力に問題があります！')
        return
      }
      if (this.stateItem[0] === this.nowState) {
        this.nowState = this.stateItem[1]
      } else if (this.stateItem[1] === this.nowState) {
        this.nowState = this.stateItem[2]
      } else if (this.stateItem[2] === this.nowState) {
        this.nowState = this.stateItem[3]
      }
      this.$refs.stateFullBtn.changeRejectState()
    },
  },
}
</script>

<style scoped lang="scss">
.section {
  height: 70vh;
  align-items: center;
  justify-content: center;
}
.file-selector {
  width: 300px;
  height: 300px;
}
#navigation-btn {
  position: fixed;
  bottom: 100px;
  right: 100px;
}
</style>
