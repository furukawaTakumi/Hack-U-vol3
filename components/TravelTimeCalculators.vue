<template>
  <div>
    <v-container>
      <v-row align="center">
        <v-col>
          <google-maps
            ref="originMap"
            :placeholder="placeholders.origin"
            @geoCording="updateOriginLocation"
          />
        </v-col>
        <v-col class="arrow-col">
          <arrow-signs ref="arrowSign" />
        </v-col>
        <v-col>
          <google-maps
            ref="destinationMap"
            :placeholder="placeholders.destination"
            @geoCording="updateDestinationLocation"
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
      <v-card
        v-if="
          updateState.origin === updateState.destination &&
          updateState.origin === 'OK' &&
          responseState === 'OK'
        "
      >
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
      responseState: 'MAP_ERROR',
      placeholders: { origin: '現在地', destination: 'データ送信先住所' },
      addressTexts: { origin: null, destination: null },
      locations: { origin: null, destination: null },
      updateState: { origin: 'NULL', destination: 'NULL' },
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
      if (this.responseState === 'OK') return ''
      if (this.responseState === 'NOT_FOUND') {
        return `${this.placeholders.origin}、または${this.placeholders.destination}が見つかりませんでした。`
      }
      if (this.responseState === 'ZERO_RESULTS') {
        return `ルートが見つかりませんでした。`
      }
      if (this.responseState === 'UNKNOWN_ERROR') {
        return 'サーバエラーが発生しました。一定時間後にもう一度お試しください。'
      }
      if (this.responseState === 'MAP_ERROR')
        return `${this.placeholders.origin}、または、${this.placeholders.destination}の入力に問題があります。`
      return '想定外のエラーが発生'
    },
  },
  mounted() {
    window.addEventListener('load', () => {
      this.service = new google.maps.DirectionsService() // eslint-disable-line
    })
  },
  methods: {
    updateOriginLocation(updateObj) {
      this.updateState.origin = updateObj.status
      if (updateObj.status === 'OK') {
        this.addressTexts.origin = updateObj.address
        this.locations.origin = updateObj.location
        if (
          'OK' === updateObj.status &&
          updateObj.status === this.updateState.destination
        ) {
          this.calcuTravelTime()
        }
      } else {
        this.changeToInvalidState()
      }
    },
    updateDestinationLocation(updateObj) {
      this.updateState.destination = updateObj.status
      if (updateObj.status === 'OK') {
        this.addressTexts.destination = updateObj.address
        this.locations.destination = updateObj.location
        if (
          'OK' === updateObj.status &&
          updateObj.status === this.updateState.origin
        ) {
          this.calcuTravelTime()
        }
      } else {
        this.changeToInvalidState()
      }
    },
    calcuTravelTime() {
      this.$refs.arrowSign.startAnimation()
      this.service.route(
        {
          origin: this.locations.origin,
          destination: this.locations.destination,
          travelMode: 'DRIVING',
        },
        (response) => {
          this.responseState = response.status
          if (response.status === 'OK') {
            this.durationObj = response.routes[0].legs[0].duration
            this.distanceObj = response.routes[0].legs[0].distance
            this.$emit('calcu-travel-time', {
              status: this.isAPIAllOk(),
              duration: this.durationObj,
            })
          } else {
            console.error('結果が取得できませんでした')
            this.$emit('calcu-travel-time', {
              status: this.isAPIAllOk(),
              duration: null,
            })
          }
        }
      )
      this.$refs.arrowSign.stopAnimation()
    },
    changeToInvalidState() {
      this.responseState = 'MAP_ERROR'
      this.addressTexts.destination = ''
      this.locations.destination = null
      this.$emit('calcu-travel-time', false)
    },
    isAPIAllOk() {
      return (
        this.responseState == 'OK' &&
        this.updateState.origin == 'OK' &&
        this.updateState.destination == 'OK'
      )
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
