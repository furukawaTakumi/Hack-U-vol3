<template>
  <div :id="googleMapId" class="google-map" />
</template>

<script>
export default {
  data () {
    return {
      googleMapId: this.$uuid.v4(),
      googleMap: null
    }
  },
  mounted () {
    window.addEventListener('load', () => {
      const mapElem = document.getElementById(this.googleMapId)
      /* eslint-disable */ // google の変数が未定義エラーとして怒られるため。
      this.googleMap = new google.maps.Map(mapElem, {
        center: { lat: -34.397, lng: 150.644 },
        zoom: 4
      } )
      /* eslint-enable */
    })
  },
  methods: {
  },
  head () {
    return {
      script: [
        { src: `https://maps.googleapis.com/maps/api/js?key=${process.env.NUXT_ENV_GMAP_API_KEY}` }
      ]
    }
  }
}
</script>

<style scoped>
.google-map {
  /* 大きさは親要素で指定 */
  width: 100%;
  height: 100%;
}
</style>
