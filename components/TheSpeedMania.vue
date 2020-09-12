<template>
  <div>
    <div id="in-test-value">
      <in-test-value />
    </div>
    <v-container>
      <v-row
        v-show="stateItem[0].value <= nowState.value"
        id="section-1"
        class="section"
      >
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
      <v-divider inset />
      <v-row
        v-show="stateItem[1].value <= nowState.value"
        id="section-2"
        class="section"
      >
        <div class="travel-time-calc-section">
          <h5 class="travel-time-calc-label">
            現在地点・データ送信先入力してください。
          </h5>
          <travel-time-calculators @calcu-travel-time="updateTravelTime" />
        </div>
      </v-row>
      <v-divider inset />
      <v-row v-show="isInputsAllFilled" class="section">
        <result ref="results" />
      </v-row>
      <v-row v-show="isInputsAllFilled" justify="center" class="result-section">
        <!-- result コンポーネントをここに入れる -->
        <speed-test
          id="speedTest"
          ref="speedTest"
          @finish-speed-test="setSpeedTestResult"
        />
      </v-row>
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
import Result from '@/components/Result'
import InTestValue from '@/components/InTestValue'

export default {
  components: {
    TravelTimeCalculators,
    FileSizeInputs,
    FileSelectors,
    SpeedTest,
    StateFullButtons,
    Result,
    InTestValue,
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
      isFinishedSpeedTest: false,
    }
  },
  computed: {
    isInputsAllFilled() {
      try {
        this.setResult()
      } catch {
        console.log('none loaded')
      }
      return (
        this.inputsData.fileSize > 0 &&
        this.inputsData.travelTime > 0 &&
        this.inputsData.speedValue > 0
      )
    },
  },
  mounted() {
    this.$refs.stateFullBtn.changeRejectState()
    this.$refs.speedTest.start()
  },
  methods: {
    updateFileSize(fileSize) {
      if (fileSize === 0) {
        this.$refs.stateFullBtn.changeRejectState()
        this.inputsData.fileSize = null
      } else {
        this.$refs.stateFullBtn.changeAcceptState()
        this.inputsData.fileSize = fileSize
      }
    },
    updateTravelTime(travelTimeObj) {
      if (travelTimeObj.status) {
        this.$refs.stateFullBtn.changeAcceptState()
        this.inputsData.travelTime = travelTimeObj.duration.value
      } else {
        this.$refs.stateFullBtn.changeRejectState()
        this.inputsData.travelTime = null
      }
    },
    setSpeedTestResult(speedValue) {
      this.inputsData.speedValue = speedValue
    },
    proceedInput(btnState) {
      if (!btnState) {
        alert('入力に問題があります！')
        return
      }
      if (this.stateItem[0].value === this.nowState.value) {
        this.nowState = this.stateItem[1]
        this.$nextTick(() => {
          this.scrollNextInputs(this.stateItem[1].value)
        })
      } else if (this.stateItem[1].value === this.nowState.value) {
        this.nowState = this.stateItem[2]
      }
      this.$refs.stateFullBtn.changeRejectState()
    },
    scrollNextInputs(number) {
      const id = `section-${number}`
      const element = document.getElementById(id)
      const moveVal =
        window.pageYOffset + element.getBoundingClientRect().bottom
      window.scrollTo(0, moveVal)
    },
    setResult() {
      this.$refs.results.resultJudge(
        this.inputsData.fileSize,
        this.inputsData.speedValue,
        this.inputsData.travelTime
      )
    },
  },
}
</script>

<style scoped lang="scss">
.centerling {
  align-items: center;
  justify-content: center;
}
.section {
  height: 70vh;
  @extend .centerling;
}
.result-section {
  @extend .centerling;
  align-items: flex-end;
  height: 4em;
  padding: 30px;
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
  bottom: 180px;
  left: 250px;
}
#in-test-value {
  position: relative;
  background-color: rgb(179, 179, 179);
  transform: rotateZ(-90deg);
  width: 100vh;
  height: 150px;
  left: calc(100vh - 150px);
}
</style>
