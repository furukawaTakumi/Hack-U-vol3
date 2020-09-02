<template>
  <dev>
    <script type="text/javascript" src="speedtest.js"></script>
    <h1>LibreSpeed Example</h1>

    <h4>IP Address</h4>
    <p id="ip"></p>

    <h4>Download</h4>
    <p id="download"></p>

    <h4>Upload</h4>
    <p id="upload"></p>

    <h4>Latency</h4>
    <p id="ping"></p>

    <script type="text/javascript">
      var s = new Speedtest();
      s.onupdate = function(data) {
        document.getElementById("download").textContent = data.dlStatus + " Mbit/s";
        document.getElementById("upload").textContent = data.ulStatus + " Mbit/s";
        document.getElementById("ping").textContent =
          data.pingStatus + " ms, " + data.jitterStatus + " ms jitter";
        document.getElementById("ip").textContent = data.clientIp;
      };
      s.start();
    </script>

    <a href="https://github.com/librespeed/speedtest">Source code</a>
  </dev>
</template>
