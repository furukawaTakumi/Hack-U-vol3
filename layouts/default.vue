<template>
  <div>
    <TheArrowSigh />
    <Nuxt />
    <the-footer />
  </div>
</template>
<script>
import TheArrowSigh from '@/components/TheArrowSigh'
import TheFooter from '@/components/TheFooter'
export default {
  components: {
    TheArrowSigh,
    TheFooter
  }
}
</script>
