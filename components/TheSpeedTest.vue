<template>
  <div>
    <!-- <script src="./speedtest.js" /> -->
    <h1>LibreSpeed Example</h1>

    <h4>Download</h4>
    <p id="download" />

    <h4>Upload</h4>
    <p id="upload" />

    <h4>Latency</h4>
    <p id="ping" />

    <a href="https://github.com/librespeed/speedtest">Source code</a>
  </div>
</template>

<script>
export default {
  mounted () {
  /* eslint-disable */
    window.addEventListener('load', () => {
    const s = new Speedtest()
    s.onupdate = function (data) { // when status is received, put the values in the appropriate fields
      document.getElementById('download').textContent = data.dlStatus + ' Mbps'
      document.getElementById('upload').textContent = data.ulStatus + ' Mbps'
      document.getElementById('ping').textContent = data.pingStatus + ' ms '
    }
    s.start() // start the speedtest with default settings
    })
  },
  head () {
    return {
      script: [
        { src: 'speedtest.js' },
        { src: 'speedtest_worker.js'}
      ]
    }
  }
}
</script>
