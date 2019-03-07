<template>
  <canvas id="app"></canvas>
</template>

<script>
import { Deck } from '@deck.gl/core'
import { GeoJsonLayer } from '@deck.gl/layers'

export default {
  name: 'app',
  data () {
    return {
      deck: ''
    }
  },
  methods: {
    deck_geojson: function () {
      const GEOJSON =
        'https://d2ad6b4ur7yvpq.cloudfront.net/naturalearth-3.3.0/ne_110m_admin_1_states_provinces_shp.geojson'
      const INITIAL_VIEW_STATE = {
        latitude: 35,
        longitude: -115,
        zoom: 3,
        bearing: 30,
        pitch: 30
      }
      this.deck = new Deck({
        canvas: 'app',
        initialViewState: INITIAL_VIEW_STATE,
        controller: true,
        layers: [
          new GeoJsonLayer({
            visible: true,
            data: GEOJSON,
            stroked: true,
            filled: true,
            lineWidthMinPixels: 2,
            opacity: 0.4,
            getLineColor: () => [255, 100, 100],
            getFillColor: () => [200, 160, 0, 180]
          })
        ]
      })
    },
    deckInit: function () {
      this.deck_geojson()
    },
    deckDestory () {
      this.deck.finalize()
    }
  },
  mounted () {
    this.deckInit()
  },
  beforeDestroy () {
    this.deckDestory()
  }
}
</script>

<style>
body {
  margin: 0;
}
</style>
