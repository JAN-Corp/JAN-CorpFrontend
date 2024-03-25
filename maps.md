---
permalink: /maps
title: maps
---

<html style="height: 100%; margin: 0; padding: 0;">
<head>
  <title>Real Estate Map</title>
  <style>
    #map-container {
      height: 80vh; /* Adjust height as needed */
      width: 80%; /* Adjust width as needed */
      margin: 20px auto; /* Center the map horizontally */
      border: 2px solid #ccc; /* Add border */
      border-radius: 10px; /* Optional: Add border radius */
    }
    #map {
      height: 100%;
      width: 100%;
    }
  </style>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=default"></script>

<script type="text/javascript">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    let map;

    async function initMap() {
      map = new google.maps.Map(document.getElementById("map"), {
        center: { lat: 33.000, lng: -117.200 }, // Centered at Southern California
        zoom: 8, // Adjust the zoom level as needed
        styles: [] // Remove custom styles
      });

      casync function fetchHouseData() {
        try {
          const url = uri + '/api/house/houses';
          const response = await fetch(url);
          const data = await response.json();
          return data;
        } catch (error) {
          console.error('Error fetching house data:', error);
          return [];
        }
      }

      async function addMarkers() {
        const housesData = await fetchHouseData();
        housesData.forEach(house => {
          const { lat, long, address } = house;
          addMarkerWithInfo(address, parseFloat(lat), parseFloat(long));
        });
      }

      function addMarkerWithInfo(address, lat, lng) {
        const marker = new google.maps.Marker({
          position: { lat, lng },
          map: map,
          title: address
        });

        marker.addListener('click', function() {
          alert('Place: ' + address + '\nLatitude: ' + lat + '\nLongitude: ' + lng);
        });
      }

      addMarkers();
    }
  </script>

<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyC9pEqmLuhD8-W86bZHiuDpCgeXlCoxVTc&callback=initMap" async defer></script>

</head>
<body>
  <!--The div element for the map -->
  <div id="map-container">
    <div id="map"></div>
  </div>
</body>
</html>
