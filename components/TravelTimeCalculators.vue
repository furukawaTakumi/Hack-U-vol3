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
    <v-dialog v-model="isOpenedModal" class="detail-modal">
      <v-card>
        <v-card-title>移動時間の詳細</v-card-title>
        <v-card-text>
          <v-simple-table>
            <tr>
              <td>移動距離</td>
              <td>{{ distanceDetailText }}</td>
            </tr>
            <tr>
              <td>移動時間</td>
              <td>{{ durationDetailText }}</td>
            </tr>
          </v-simple-table>
        </v-card-text>
      </v-card>
    </v-dialog>
    <p>移動距離: {{ distanceObj.text }}</p>
    <p>移動時間: {{ durationObj.text }}</p>
    <button @click="calcuTravelTime(); isOpenedModal = true">距離の計算する</button>
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
      distanceObj: { distance: 0, text: '' },
      durationObj: { duration: 0, text: '' },
      isOpenedModal: false
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
    distanceDetailText () {
      return `${this.distanceObj.value / 1000} km`
    },
    durationDetailText () {
      return `${Math.floor(this.durationObj.value / 60)} 分`
    }
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

<style lang='scss' scoped>
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
