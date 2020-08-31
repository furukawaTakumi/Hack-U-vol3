<template>
  <div id="map"></div>
</template>

<script>

export default {
  data () {
    return {
      googleMap: null
    }
  },
  mounted () {
    window.initMap = function () {
      /* eslint-disable */ // google の変数が未定義エラーとして怒られるため。
      this.googleMap = new google.maps.Map(document.getElementById('map'), { 
        center: { lat: -34.397, lng: 150.644 },
        zoom: 4
      })
      /* eslint-enable */
    }
  },
  methods: {
    private () {
      return {
      }
    }
  },
  head () {
    return {
      script: [
        { src: `https://maps.googleapis.com/maps/api/js?key=${process.env.NUXT_ENV_GMAP_API_KEY}&callback=initMap`, defer: true }
      ]
    }
  }
}
</script>

<style scoped>
#map {
  width: 300px;
  height: 300px;
}
</style>
