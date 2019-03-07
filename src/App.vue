<template>
  <div id="app">
  </div>
</template>

<script>
import { MapboxLayer } from '@deck.gl/mapbox'
import { ScatterplotLayer } from '@deck.gl/layers'
import mapboxgl from 'mapbox-gl'
import 'mapbox-gl/dist/mapbox-gl.css'

export default {
  name: 'app',
  data () {
    return {}
  },
  methods: {
    deck_geojson_mapboxgl: function () {
      mapboxgl.accessToken = 'pk.eyJ1IjoidWJlcmRhdGEiLCJhIjoiY2pudzRtaWloMDAzcTN2bzN1aXdxZHB5bSJ9.2bkj3IiRC8wj3jLThvDGdA'

      const map = new mapboxgl.Map({
        container: 'app',
        style: 'mapbox://styles/mapbox/light-v9',
        center: [-74.50, 40],
        zoom: 9
      })

      const myDeckLayer = new MapboxLayer({
        id: 'my-scatterplot',
        type: ScatterplotLayer,
        data: [
          { position: [-74.5, 40], size: 10000 }
        ],
        getPosition: d => d.position,
        getRadius: d => d.size,
        getFillColor: [255, 0, 0],
        getLineColor: [0, 0, 0]
      })

      map.on('load', () => {
        map.addLayer(myDeckLayer, 'waterway-label')
      })
    },
    deckInit: function () {
      this.deck_geojson_mapboxgl()
    }
  },
  mounted () {
    this.deckInit()
  },
  beforeDestroy () {}
}
</script>

<style>
html,body,#app {
  margin: 0;
  width: 100%;
  height: 100%;
}
</style>
