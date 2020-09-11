<template>
  <div class="map-frame">
    <div :id="googleMapId" class="google-map" />
    <v-text-field
      v-model="addressText"
      solo
      single-line
      class="text-field"
      :placeholder="placeholder"
      @change="geoCording()"
    />
    <div class="message" :class="messageColor">
      {{ statusMessage }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    placeholder: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      googleMapId: this.$uuid.v4(),
      googleMap: null,
      geocoder: null,
      addressText: '',
      location: null,
      googleLib: null,
      marker: null,
      responseStatus: 'NULL',
      messageColor: 'red--text',
    }
  },
  computed: {
    statusMessage() {
      if ('OK' === this.responseStatus) return '入力完了。'
      if (
        'NULL' === this.responseStatus ||
        'INVALID_REQUEST' === this.responseStatus
      )
        return '入力してください。'
      if ('ZERO_RESULTS' === this.responseStatus)
        return '住所に変換できません。'
      if (
        'UNKNOWN_ERROR' === this.responseStatus ||
        'ERROR' === this.responseStatus
      )
        return 'リクエストに失敗しました。再試行してください'
      return '予期せぬエラーが発生しました。'
    },
  },
  mounted() {
    window.addEventListener('load', () => {
      this.initMap()
    })
  },
  methods: {
    initMap() {
      const mapElem = document.getElementById(this.googleMapId) // google の変数が未定義エラーとして怒られるため。
      /* eslint-disable */
      const initLocation = { lat: 35.4122, lng: 139.413 }
      this.googleLib = google
      this.googleMap = new this.googleLib.maps.Map(mapElem, {
        center: initLocation,
        zoom: 4,
      })
      this.geocoder = new this.googleLib.maps.Geocoder()
      this.marker = new this.googleLib.maps.Marker({ position: initLocation })
      this.marker.setMap(this.googleMap)
      /* eslint-enable */
    },
    geoCording() {
      this.geocoder.geocode(
        { address: this.addressText },
        (response, status) => {
          this.responseStatus = status
          if (status === 'OK') {
            const location = response[0].geometry.location.toJSON()
            this.setLocation(location)
            this.$emit('geoCording', {
              status,
              location,
              address: this.addressText,
            })
            this.messageColor = ''
          } else {
            console.error('一致する住所がありません')
            this.$emit('geoCording', { status, location: null, address: null })
            this.messageColor = 'red--text'
          }
        }
      )
    },
    getLocation() {
      return this.location
    },
    getAddressText() {
      return this.addressText
    },
    setLocation(location) {
      this.location = location
      this.googleMap.setCenter(location)
      this.marker.setMap(null)
      this.marker = new this.googleLib.maps.Marker({ position: location })
      this.marker.setMap(this.googleMap)
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

<style scoped>
.map-frame {
  width: 220px;
}
.google-map {
  height: 200px;
}
.text-field {
  margin-top: 0.4em;
}
.message {
  margin-left: 10px;
  position: relative;
  top: -20px;
  font-size: 0.4em;
}
</style>
