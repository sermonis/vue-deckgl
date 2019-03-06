## 在Vue项目使用deck.gl

>deck.gl核心库和层与React或Mapbox GL无依赖关系，可供任何JavaScript应用程序使用。master分支(当前分支)是关于独立使用的demo，mapboxgl分支则是搭配mapbox gl实现的demo。


### 安装依赖
通过yarn引入
```
yarn add @deck.gl/core @deck.gl/layers 
```
或者通过npm引入
```
npm install --save  @deck.gl/core @deck.gl/layers
```

### 引入依赖
以GeoJsonLayer为例，在xxx.vue文件中
```
<script>
  import { Deck } from '@deck.gl/core';
  import { GeoJsonLayer } from '@deck.gl/layers';
</script>
```

### 实现可视化
```
<template>
  <!-- canvas container -->
  <canvas id="app"></canvas>
</template>

<script>
  <!-- 引入依赖 -->
  import { Deck } from '@deck.gl/core';
  import { GeoJsonLayer } from '@deck.gl/layers';
  export default {
    name: 'app',
    data() {
      return {
        <!-- deck实例 -->
        deck: ''
      }
    },
    methods: {
      <!-- geojson实现 -->
      deck_geojson: function () {
        const GEOJSON =
          'https://d2ad6b4ur7yvpq.cloudfront.net/naturalearth-3.3.0/ne_110m_admin_1_states_provinces_shp.geojson';
        const INITIAL_VIEW_STATE = {
          latitude: 35,
          longitude: -115,
          zoom: 3,
          bearing: 30,
          pitch: 30
        };
        <!-- 新建Deck的实例 -->
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
        });
      },
      <!-- 初始化 -->
      deckInit: function () {
        this.deck_geojson()
      },
      <!-- 销毁 -->
      deckDestory() {
        this.deck.finalize()
      }
    },
    mounted() {
      <!-- mounted生命周期调用初始化 -->
      this.deckInit()
    },
    beforeDestroy() {
      <!-- beforeDestroy生命周期调用初始化 -->
      this.deckDestory()
    }
  }
</script>

<style>
  body {
    margin: 0;
  }
</style>
```

### 效果
![demo](https://raw.githubusercontent.com/wupeiwen/vue-deckgl/master/public/demo.png)

### 参考文档
[deck.gl API class](http://deck.gl/#/documentation/deckgl-api-reference/deck) 

[deck.gl API layers](http://deck.gl/#/documentation/deckgl-api-reference/layers/layer)

