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
      location: { lat: 35.4122, lng: 139.413 },
      googleLib: null,
      marker: null,
    }
  },
  mounted() {
    window.addEventListener('load', () => {
      this.initMap()
    })
  },
  methods: {
    initMap() {
      const mapElem = document.getElementById(this.googleMapId) // google の変数が未定義エラーとして怒られるため。
      /* eslint-disable */ this.googleLib = google
      this.googleMap = new this.googleLib.maps.Map(mapElem, {
<<<<<<< HEAD
        center: this.location,　zoom: 4
=======
        center: this.location,
        zoom: 4,
>>>>>>> master
      })
      this.geocoder = new this.googleLib.maps.Geocoder()
      this.marker = new this.googleLib.maps.Marker({ position: this.location })
      this.setLocation(this.location)
      /* eslint-enable */
<<<<<<< HEAD
    })
  },
  methods: {
    geoCording () {
      this.geocoder.geocode({ address: this.addressText }, (response, status) => {
        if (status === 'OK') {
          const location = response[0].geometry.location.toJSON()
          this.setLocation(location)
        } else {
          console.error('一致する住所がありません')
=======
    },
    geoCording() {
      this.geocoder.geocode(
        { address: this.addressText },
        (response, status) => {
          if (status === 'OK') {
            const location = response[0].geometry.location.toJSON()
            this.setLocation(location)
            this.$emit('geoCording')
          } else {
            console.error('一致する住所がありません')
          }
>>>>>>> master
        }
      )
    },
<<<<<<< HEAD
    reverseGeoCording (location) {
      console.log(location)
      this.geocoder.geocode({ location }, (results, status) => {
        console.log(results)
        console.log(status)
        if (status === 'OK') {
        }
      })
    },
    getLocation () {
=======
    getLocation() {
>>>>>>> master
      return this.location
    },
    setLocation(location) {
      this.location = location
      this.reverseGeoCording(location)
      this.googleMap.setCenter(location)
      this.marker.setMap(null)
      this.marker = new this.googleLib.maps.Marker({ position: location })
      this.marker.setDraggable(true)
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
  width: 540px;
}
.google-map {
  height: 400px;
}
.text-field {
  margin-top: 0.4em;
}
</style>
