<template>
  <dev>
    <!-- <script src="./speedtest.js" /> -->
    <h1>LibreSpeed Example</h1>

    <h4>IP Address</h4>
    <p id="ip" />

    <h4>Download</h4>
    <p id="download" />

    <h4>Upload</h4>
    <p id="upload" />

    <h4>Latency</h4>
    <p id="ping" />

    <a href="https://github.com/librespeed/speedtest">Source code</a>
  </dev>
</template>

<script src="./speedtest.js">
export default {
  methods: {
    data () {
      return {
        s: ''
      }
    },
    test () {
      this.s.onupdate = function (data) { // when status is received, put the values in the appropriate fields
        document.getElementById('download').textContent = data.dlStatus + ' Mbit/s'
        document.getElementById('upload').textContent = data.ulStatus + ' Mbit/s'
        document.getElementById('ping').textContent = data.pingStatus + ' ms, ' + data.jitterStatus + ' ms jitter'
        document.getElementById('ip').textContent = data.clientIp
      }
      this.s.start() // start the speedtest with default settings
    }
  }
}
</script>
