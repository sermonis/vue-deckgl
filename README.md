## 在Vue项目使用deck.gl

>deck.gl核心库和层与React或Mapbox GL无依赖关系，可供任何JavaScript应用程序使用。mapboxgl分支(当前分支)则是搭配mapbox gl实现的demo，master分支是关于独立使用的demo，。


### 安装依赖
通过yarn引入
```
yarn add @deck.gl/core @deck.gl/layers mapbox-gl
```
或者通过npm引入
```
npm install --save  @deck.gl/core @deck.gl/layers mapbox-gl
```

### 引入依赖
以ScatterplotLayer为例，在xxx.vue文件中
```
<script>
  import { MapboxLayer } from '@deck.gl/mapbox'
  import { ScatterplotLayer } from '@deck.gl/layers'
  import mapboxgl from 'mapbox-gl'
  import 'mapbox-gl/dist/mapbox-gl.css'
</script>
```

### 实现可视化
```
<template>
  <!-- mapbox container -->
  <div id="app"></div>
</template>

<script>
  <!-- 引入依赖 -->
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

        <!-- 新建mapboxgl实例 -->
      const map = new mapboxgl.Map({
        container: 'app',
        style: 'mapbox://styles/mapbox/light-v9',
        center: [-74.50, 40],
        zoom: 9
      })

        <!-- 新建layer实例 -->
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
        <!-- 添加layer -->
        <!-- Adding a 3D deck layer -->
        <!-- map.addLayer(layer) -->
        <!-- Inserting a 2D deck layer before an existing mapbox layer -->
        <!-- map.addLayer(layer, before?) -->
        <!-- 此处参考https://docs.mapbox.com/mapbox-gl-js/api/#map#addlayer -->
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
```

### 效果
![demo](https://raw.githubusercontent.com/wupeiwen/vue-deckgl/mapboxgl/public/demo.png)

### 参考文档
[将deck.gl与Mapbox GL一起使用](http://deck.gl/#/documentation/getting-started/using-with-mapbox) 

[@deck.gl/mapbox](http://deck.gl/#/documentation/mapbox-api-reference-experimental/overview)

