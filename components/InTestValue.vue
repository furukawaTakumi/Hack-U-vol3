<template>
<div id="in-test-frame">
  <!-- <div class="frame">{{ inTestText }}</div> -->
  <marquee id="text"> {{ inTestText }} </marquee>
</div>
</template>

<script>
export default {
  data: () => ({
    valueText: "",
    inTestText: "回線速度測定中",
    finishedText: "",
    inTest: false
  }),
  methods: {
    setTestFlag(flag) {
      if (flag) {
        this.valueText = this.inTestText;
      } else {
        this.valueText = this.finishedText;
      }
    }
  }
}
</script>

<style scoped>
#in-test-frame {
    height: 100px;
    width : 100%;
}
#text {
    font-size : 32pt
}
</style>
