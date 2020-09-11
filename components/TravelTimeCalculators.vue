<template>
  <div>
    <v-container>
      <v-row align="center">
        <v-col>
          <google-maps
            ref="originMap"
            :placeholder="placeholders.origin"
            @geoCording="calcuTravelTime()"
          />
        </v-col>
        <v-col class="arrow-col">
          <arrow-signs ref="arrowSign" />
        </v-col>
        <v-col>
          <google-maps
            ref="destinationMap"
            :placeholder="placeholders.destination"
            @geoCording="calcuTravelTime()"
          />
        </v-col>
      </v-row>
    </v-container>
    <v-container
      light-blue--text
      text--lighten-4
      text-center
      @click="isOpenedModal = !isOpenedModal"
    >
      △移動時間の詳細を表示する
    </v-container>
    <v-dialog v-model="isOpenedModal" class="detail-modal">
      <v-card v-if="responseState === 'OK'">
        <v-card-title>移動時間の詳細</v-card-title>
        <v-card-text>
          <v-simple-table>
            <tr>
              <td>{{ placeholders.origin }}</td>
              <td>{{ addressTexts.origin }}</td>
            </tr>
            <tr>
              <td>{{ placeholders.destination }}</td>
              <td>{{ addressTexts.destination }}</td>
            </tr>
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
      <v-card v-else>
        <v-card-title>エラーメッセージ</v-card-title>
        <v-card-text>
          {{ errorText }}
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
      responseState: 'NULL',
      placeholders: { origin: '現在地', destination: 'データ送信先住所' },
      addressTexts: { origin: null, destination: null },
      isOpenedModal: false,
    }
  },
  computed: {
    distanceDetailText() {
      if (this.responseState !== 'OK') return ''
      return `${this.distanceObj.value / 1000} km`
    },
    durationDetailText() {
      if (this.responseState !== 'OK') return ''
      const hours = Math.floor(this.durationObj.value / 3600)
      const minits = Math.floor((this.durationObj.value - hours * 3600) / 60)
      const seconds = Math.floor(
        this.durationObj.value - hours * 3600 - minits * 60
      )
      return `${hours}時間 ${minits}分 ${seconds}秒`
    },
    errorText() {
      if (this.responseState === 'NULL') {
        return `${this.placeholders.origin}、${this.placeholders.destination}が未入力です。`
      }
      if (this.responseState === 'NOT_FOUND') {
        return `${this.placeholders.origin}、または${this.placeholders.destination}が見つかりませんでした。`
      }
      if (this.responseState === 'ZERO_RESULTS') {
        return `ルートが見つかりませんでした。`
      }
      if (this.responseState === 'UNKNOWN_ERROR') {
        return 'サーバエラーが発生しました。一定時間後にもう一度お試しください。'
      }
      return '想定外のエラーが発生'
    },
  },
  mounted() {
    window.addEventListener('load', () => {
      this.service = new google.maps.DirectionsService() // eslint-disable-line
    })
  },
  methods: {
    calcuTravelTime() {
      this.$refs.arrowSign.startAnimation()
      this.addressTexts.origin = this.$refs.originMap.getAddressText()
      this.addressTexts.destination = this.$refs.destinationMap.getAddressText()
      this.service.route(
        {
          origin: this.$refs.originMap.getLocation(),
          destination: this.$refs.destinationMap.getLocation(),
          travelMode: 'DRIVING',
        },
        (response) => {
          this.responseState = response.status
          if (response.status === 'OK') {
            this.durationObj = response.routes[0].legs[0].duration
            this.distanceObj = response.routes[0].legs[0].distance
            this.$emit('calcu-travel-time', {
              status: this.responseState,
              duration: this.durationObj,
            })
          } else {
            console.error('結果が取得できませんでした')
            this.$emit('calcu-travel-time', {
              status: this.responseState,
              duration: this.durationObj,
            })
          }
        }
      )
      this.$refs.arrowSign.stopAnimation()
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
