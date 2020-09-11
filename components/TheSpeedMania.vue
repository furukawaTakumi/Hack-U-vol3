<template>
  <div>
    <v-container>
      <v-row v-show="stateItem[0].value <= nowState.value" class="section">
        <div class="file-size-input-section">
          <file-size-inputs
            v-if="inputFileFaild"
            ref="fileSizeInput"
            @input="updateFileSize"
          />
          <file-selectors
            v-else
            :show-file-chip-count="5"
            class="file-selector"
          />
        </div>
      </v-row>
      <v-row v-show="stateItem[1].value <= nowState.value" class="section">
        <div class="travel-time-calc-section">
          <h5 class="travel-time-calc-label">
            現在地点・データ送信先入力してください。
          </h5>
          <travel-time-calculators @calcu-travel-time="updateTravelTime" />
        </div>
      </v-row>
      <v-row v-show="stateItem[2].value <= nowState.value" class="section">
        <div class="speed-test-section">
          <h5 class="speed-test-label">回線速度を計測してください。</h5>
          <speed-test @finish-speed-test="updateLineSpeed" />
        </div>
      </v-row>
      <v-row v-show="stateItem[3].value <= nowState.value" />
    </v-container>
    <div v-if="isInputProcessing" id="navigation-btn">
      <state-full-buttons ref="stateFullBtn" @click="proceedInput" />
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
  data() {
    return {
      nowState: { text: 'no-input', value: 1 },
      inputFileFaild: true,
      stateItem: [
        { text: 'no-input', value: 1 },
        { text: 'confirmed file size', value: 2 },
        { text: 'confirmed travel time ', value: 3 },
        { text: 'finished speed test ', value: 4 },
      ],
      inputsData: {
        fileSize: null,
        travelTime: null,
        speedValue: null,
      },
      isInputProcessing: true,
    }
  },
  mounted() {
    this.$refs.stateFullBtn.changeRejectState()
  },
  methods: {
    updateFileSize(fileSize) {
      if (fileSize === 0) {
        this.$refs.stateFullBtn.changeRejectState()
        this.fileSize = null
      } else {
        this.$refs.stateFullBtn.changeAcceptState()
        this.fileSize = fileSize
      }
    },
    updateTravelTime(travelTimeObj) {
      if (travelTimeObj.status === 'OK') {
        this.$refs.stateFullBtn.changeAcceptState()
        this.inputsData.travelTime = travelTimeObj.duration.value
      } else {
        this.$refs.stateFullBtn.changeRejectState()
        this.inputsData.travelTime = null
      }
    },
    updateLineSpeed(speedValue) {
      this.inputsData.travelTime = speedValue
      this.$refs.stateFullBtn.changeAcceptState()
      this.$emit('all-input-finished', this.inputsData)
      this.isInputProcessing = false
    },
    proceedInput(btnState) {
      if (!btnState) {
        alert('入力に問題があります！')
        return
      }
      if (this.stateItem[0].value === this.nowState.value) {
        this.nowState = this.stateItem[1]
      } else if (this.stateItem[1].value === this.nowState.value) {
        this.nowState = this.stateItem[2]
      } else if (this.stateItem[2].value === this.nowState.value) {
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
.travel-time-calc-label {
  font-size: 2em;
  text-align: center;
  margin: 20px;
}
.speed-test-label {
  font-size: 2em;
  text-align: center;
  margin: 20px 20px 40px;
}
#navigation-btn {
  position: fixed;
  bottom: 200px;
  right: 400px;
}
</style>
