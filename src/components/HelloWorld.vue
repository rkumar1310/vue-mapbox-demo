<template>
  <div id="map"></div>
</template>

<script>
const mapboxgl = require('mapbox-gl/dist/mapbox-gl.js')
const axios = require('axios').default

export default {
  name: 'App',
  components: {},
  data() {
    return {
      accessToken:
        'pk.eyJ1Ijoicmt1bWFyMTMxMCIsImEiOiJja25qZGY3dnkwMDQ4MnZueG5zbmhicnc1In0.eTr-OUJu1qVjdn0APX-y3Q',
      mapStyle: 'mapbox://styles/mapbox/streets-v11',
      layerId: 'firstLayer',
      sourceId: 'firstSource',
      coordinates: [-74.5, 40],
      center: [-74.5, 40],
      zoom: 10,
      markers: [],
    }
  },
  mounted() {
    mapboxgl.accessToken =
      'pk.eyJ1Ijoicmt1bWFyMTMxMCIsImEiOiJja25qZDR1bncwMDBlMm5wOHNkdGs2Y3ZjIn0.ME3c0Lu9x7JPrzgWFU6lpg'
    var map = new mapboxgl.Map({
      container: 'map', // container id
      style: 'mapbox://styles/mapbox/streets-v11', // style URL
      center: [-74.5, 40], // starting position [lng, lat]
      zoom: 9, // starting zoom
    })
    axios.get('/markers.json').then(({ data }) => {
      data.forEach((marker) => {
        // Create a default Marker and add it to the map.
        
        // create the popup
        var popup = new mapboxgl.Popup({ offset: 25 }).setText(
          marker.address + ' / ' + marker.report_type
        )
        new mapboxgl.Marker().setLngLat(marker.point.coordinates).setPopup(popup).addTo(map)
      })
    })
  },
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}
</style>
