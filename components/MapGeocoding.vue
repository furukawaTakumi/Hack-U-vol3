<template>
  <body>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=default" />
    <script
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCPJpjD-qcR_yIxJnS8maR5W9KB0E3EzYI&callback=initMap&libraries=&v=weekly"
      defer
    ></script>
    <div id="floating-panel">
      <input id="address" type="textbox" value="Sydney, NSW" />
      <input id="submit" type="button" value="Geocode" />
    </div>
    <div id="map"></div>
  </body>

</template>

<script>
export default {
}
import index from '@/static/index.js'
export default {
    data: () => {

    },
  head () {
    return {
      script: [
        { src: 'index.js' }
      ]
    }
  }
  </script>



    <style type="text/css">
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
