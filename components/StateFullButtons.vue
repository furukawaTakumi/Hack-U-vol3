<template>
  <v-btn
    :color="colorString"
    rounded
    dark
    class="state-full-btn"
    :class="{ 'is-accept-color': isAccept, 'is-not-accept-color': !isAccept }"
    @click="$emit('click', isAccept)"
  >
    {{ textString }}
  </v-btn>
</template>

<script>
export default {
  data() {
    return {
      state: { isAccept: true, text: 'Next' },
      stateString: 'Next',
      isAccept: true,
    }
  },
  computed: {
    colorString() {
      if (this.isAccept) {
        return 'teal'
      } else {
        return 'error'
      }
    },
    textString() {
      if (this.isAccept) {
        return 'Next'
      } else {
        return '---'
      }
    },
  },
  methods: {
    changeAcceptState() {
      this.isAccept = true
    },
    changeRejectState() {
      this.isAccept = false
    },
  },
}
</script>

<style scoped lang="scss">
.state-full-btn {
  width: 100px;
}
.is-accept-color {
  background-color: chartreuse;
}
.is-not-accept-color {
  background-color: yellow;
}
</style>
