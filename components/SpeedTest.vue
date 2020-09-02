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
  head () {
    return {

    }
  }
}
</script>
