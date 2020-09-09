<template>
  <div>
    <v-container>
      <v-row v-show="nowState === stateItem[0]" class="section">
        <div class="file-size-input-section">
          <file-size-inputs v-if="inputFileFaild" />
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
    <div id="navigation-btn" @click="proceedInput">
      <v-btn fab dark color="teal">
        <v-icon>></v-icon>
      </v-btn>
    </div>
  </div>
</template>

<script>
import TravelTimeCalculators from '@/components/TravelTimeCalculators'
import FileSizeInputs from '@/components/FileSizeInputs'
import FileSelectors from '@/components/FileSelectors'
import SpeedTest from '@/components/SpeedTest'

export default {
  components: {
    TravelTimeCalculators,
    FileSizeInputs,
    FileSelectors,
    SpeedTest,
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
    }
  },
  methods: {
    proceedInput() {
      if (this.stateItem[0] === this.nowState) {
        this.nowState = this.stateItem[1]
      } else if (this.stateItem[1] === this.nowState) {
        this.nowState = this.stateItem[2]
      } else if (this.stateItem[2] === this.nowState) {
        this.nowState = this.stateItem[3]
      }
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
