<template>
  <div>
    <v-container>
      <v-row align="center">
        <v-col>
          <google-maps
            ref="originMap"
            @geoCording="calcuTravelTime()"
            :placeholder="'送りたいデータのある住所'"
          />
        </v-col>
        <v-col class="arrow-col">
          <arrow-signs ref="arrowSign" />
        </v-col>
        <v-col>
          <google-maps
            ref="destinationMap"
            @geoCording="calcuTravelTime()"
            :placeholder="'データを送る住所'"
          />
        </v-col>
      </v-row>
    </v-container>
    <v-container
      @click="isOpenedModal = !isOpenedModal"
      light-blue--text
      text--lighten-4
      text-center
    >
      △移動時間の詳細を表示する
    </v-container>
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
  </div>
</template>

<script>
import GoogleMaps from '@/components/TravelTimeCalculators'
import ArrowSigns from '@/components/ArrowSigns'

export default {
  components: {
    GoogleMaps,
    ArrowSigns,
  },
  data() {
    return {
      service: null,
      distanceObj: { value: 0, text: '' },
      durationObj: { value: 0, text: '' },
      isOpenedModal: false,
    }
  },
  mounted() {
    window.addEventListener('load', () => {
      this.service = new google.maps.DirectionsService() // eslint-disable-line
    })
  },
  methods: {
    calcuTravelTime() {
      this.$refs.arrowSign.startAnimation()
      this.service.route(
        {
          origin: this.$refs.originMap.getLocation(),
          destination: this.$refs.destinationMap.getLocation(),
          travelMode: 'DRIVING',
        },
        (response) => {
          if (response.status === 'OK') {
            this.durationObj = response.routes[0].legs[0].duration
            this.distanceObj = response.routes[0].legs[0].distance
          } else if (response.status === 'ZERO_RESULTS') {
            console.error('結果が取得できませんでした')
          }
        }
      )
      this.$refs.arrowSign.stopAnimation()
    },
  },
  computed: {
    distanceDetailText() {
      if (this.distanceObj.value === 0) {
        return '交通手段がない、もしくは初期値のままです。'
      }
      return `${this.distanceObj.value / 1000} km`
    },
    durationDetailText() {
      if (this.durationObj.value === 0) {
        return '交通手段がない、もしくは初期値のままです。'
      }
      const hours = Math.floor(this.durationObj.value / 3600)
      const minits = Math.floor((this.durationObj.value - hours * 3600) / 60)
      const seconds = Math.floor(
        this.durationObj.value - hours * 3600 - minits * 60
      )
      return `${hours}時間 ${minits}分 ${seconds}秒`
    },
  },
  head() {
    return {
      script: [
        {
          src: `https://maps.googleapis.com/maps/api/js?key=${process.env.NUXT_ENV_GMAP_API_KEY}`,
        },
      ],
    }
  },
}
</script>

<style lang="scss" scoped>
.frame {
  display: flex;
  flex-direction: row;
  align-items: center;
}
</style>
