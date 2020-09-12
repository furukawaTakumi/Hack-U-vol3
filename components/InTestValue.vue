<template>
  <div id="in-test-frame">
    <!-- <div class="frame">{{ inTestText }}</div> -->
    <marquee id="text">
      {{ valueText }}
    </marquee>
  </div>
</template>

<script>
export default {
  data: () => ({
    valueText: '回線速度測定中',
    inTestText: '回線速度測定中',
    finishedText: '測定完了',
    inTest: false,
  }),
  methods: {
    setTestFlag(flag) {
      console.log(flag)
      if (flag) {
        this.valueText = this.inTestText
      } else {
        this.valueText = this.finishedText
      }
    },
  },
}
</script>

<style scoped>
#in-test-frame {
  height: 100px;
  width: 100%;
}
#text {
  font-size: 32pt;
}
</style>
