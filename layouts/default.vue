<template>
  <div>
    <TheFileSelector />
    <nuxt />
    <the-footer />
  </div>
</template>

<script>
import TheFileSelector from '@/components/TheFileSelector'
import TheFooter from '@/components/TheFooter'

export default {
  components: {
    TheFileSelector,
    TheFooter
  }
}
</script>
