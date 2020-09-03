/* eslint-enable */
<template>
<html>
  <head>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=default"></script>
    <script
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyAQs7Zy7Yhm1hQbBNjxLymSPOp7bMlg1IE&callback=initMap&libraries=&v=weekly"
      defer
    ></script>
  </head>
  <body>
    <div id="floating-panel">
      <input id="address" type="textbox" value="住所を入力" />
      <input id="submit" type="button" value="submit"/>
    </div>
    <div id="map"></div>
  </body>
  <script>
  "use strict";

function initMap() {
  const map = new google.maps.Map(document.getElementById("map"), {
    zoom: 9,
    center: {
      lat: 35.1834122,
      lng: 137.1130419
    }
  });
  const geocoder = new google.maps.Geocoder();
  document.getElementById("submit").addEventListener("click", () => {
    geocodeAddress(geocoder, map);
  });
}

function geocodeAddress(geocoder, resultsMap) {
  const address = document.getElementById("address").value;
  geocoder.geocode(
    {
      address: address
    },
    (results, status) => {
      if (status === "OK") {
        resultsMap.setCenter(results[0].geometry.location);
        new google.maps.Marker({
          map: resultsMap,
          position: results[0].geometry.location
        });
      } else {
        alert("Geocode was not successful for the following reason: " + status);
      }
    }
  );
}

</script>
</html>
</template>
/* eslint-enable */

    <style type="text/css" scoped>
      /* Always set the map height explicitly to define the size of the div
       * element that contains the map. */
      #map {
        height: 100%;
      }

      /* Optional: Makes the sample page fill the window. */
      html,
      body {
        height: 400px;
        width: 450px;
        margin: 10;
        padding: 0;
      }

      #floating-panel {
        position: absolute;
        top: 100px;
        left: 40px;
        z-index: 5;
        background-color: #fff;
        padding: 4px;
        border: 1px solid #999;
        text-align: center;
        font-family: "Roboto", "Roboto";
        line-height: 23px;
        padding-left: 10px;
      }
</style>
