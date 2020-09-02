<template>
  <div>
    <the-speed-test />
    <nuxt />
    <the-footer />
  </div>
</template>

<script>
import TheFooter from '@/components/TheFooter'
import TheSpeedTest from '@/components/SpeedTest'

export default {
  components: {
    TheFooter,
    TheSpeedTest
  }
}
</script>
