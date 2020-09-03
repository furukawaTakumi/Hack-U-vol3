/* eslint-enable */
<template>
  <head>
    <title>Geocoding Service</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=default"></script>
    <script
      src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap&libraries=&v=weekly"
      defer
    ></script>
    <link rel="stylesheet" type="text/css" href="./style.css" />
    <script src="./app.js"></script>
  </head>
    <div id="floating-panel">
      <input id="address" type="textbox" value="Sydney, NSW" />
      <input id="submit" type="button" value="Geocode" />
    </div>
    <div id="map" />
</template>
<script>
export default {

}
</script>

    <style type="text/css" scoped>
      /* Always set the map height explicitly to define the size of the div
       * element that contains the map. */
      #map {
        height: 100%;
      }

      /* Optional: Makes the sample page fill the window. */
      html,
      body {
        height: 100%;
        margin: 0;
        padding: 0;
      }

      #floating-panel {
        position: absolute;
        top: 10px;
        left: 25%;
        z-index: 5;
        background-color: #fff;
        padding: 5px;
        border: 1px solid #999;
        text-align: center;
        font-family: "Roboto", "sans-serif";
        line-height: 30px;
        padding-left: 10px;
      }
</style>
