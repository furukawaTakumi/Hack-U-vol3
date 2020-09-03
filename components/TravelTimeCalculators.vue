<template>
  <div>
    <div class="frame">
      <div class="origin-map">
        <google-maps ref="originMap" />
      </div>
      <div class="arrow-sigh-range">
        <arrow-signs />
      </div>
      <div class="destination-map">
        <google-maps ref="destination_map" />
      </div>
    </div>
    <p>distance: {{ this.distanceObj.text }}</p>
    <p>duration: {{ this.durationObj.text }}</p>
    <button @click="calcuTravelTime()">距離の計算する</button>
  </div>
</template>

<script>
import GoogleMaps from '@/components/TravelTimeCalculators'
import ArrowSigns from '@/components/ArrowSigns'

export default {
  components: {
    GoogleMaps,
    ArrowSigns
  },
  data () {
    return {
      service: null,
      distanceObj: {},
      durationObj: {}
    }
  },
  mounted () {
    const instance = this
    window.addEventListener('load', () => {
      instance.service = new google.maps.DistanceMatrixService(); // eslint-disable-line
    })
  },
  methods: {
    calcuTravelTime () {
      // const origin = this.$refs.originMap.getSpot()
      // const destination = this.$refs.destinationMap.getSpot()
      /* eslint-disable */ // google 変数が未定義だと怒られるため
      const origin = new google.maps.LatLng(35.183586468431, 137.11180451123)
      const destination = new google.maps.LatLng(35.1766465, 137.1062392)
      /* eslint-enable */
      this.service.getDistanceMatrix({
        origins: [origin],
        destinations: [destination],
        travelMode: 'DRIVING'
      }, (response, status) => {
        if (status === 'OK') {
          const result = response.rows[0].elements[0]
          if (status !== 'OK') {
            throw new Error('error: element status is not OK.')
          }
          this.distanceObj = result.distance
          this.durationObj = result.duration
        } else {
          throw new Error('error: status is not OK.')
        }
      })
    }
  },
  computed: {
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
.frame {
  display: flex;
  flex-direction: row;
  width: 100%;
  align-items: center;
}
.origin-map {
  width: 300px;
  height: 180px;
}
.destination-map {
  width: 300px;
  height: 180px;
}
</style>
