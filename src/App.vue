<template>
  <div style="height: 75vh; width: 50vw;">
    <l-map ref="map" id="map" v-model="zoom" v-model:zoom="zoom" :center="[47.41322, -1.219482]" @move="log('move')">
      <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"></l-tile-layer>
      <l-control-layers />
      <l-marker :lat-lng="[0, 0]" draggable @moveend="log('moveend')">
        <l-tooltip>
          lol
        </l-tooltip>
      </l-marker>

      <!-- <l-marker :lat-lng="[47.41322, -1.219482]">
        <l-icon :icon-url="iconUrl" :icon-size="iconSize" />
      </l-marker> -->

      <l-marker :lat-lng="[50, 50]" draggable @moveend="log('moveend')">
        <l-popup>
          lol
        </l-popup>
      </l-marker>
      <!-- <l-marker :lat-lng="[47.234787, -1.358337]">
        <div style="width: 10px;height: 10px;border-radius: 50%;background-color:aquamarine;"></div>
      </l-marker> -->
      <!-- <l-marker
      v-for="(item, index) in [
        [47.334852, -1.509485],
        [47.234787, -1.358337, 0],
        [47.342596, -1.328731],
        [47.241487, -1.190568, 1],
      ]"
      :key="index"
      :lat-lng="item"
      :icon="rbPoint"
      class="mark"
    >
      <l-icon class-name="someExtraClass">
        <div class="headline">
          {{ index+1 }}
        </div>
      </l-icon>
    </l-marker> -->

    <!-- 多个点位循环绘制 -->
      <l-marker v-for="(item, index) in [
        [47.334852, -1.509485],
        [47.234787, -1.358337, 0],
        [47.342596, -1.328731],
        [47.241487, -1.190568, 1],
      ]" :key="index" :lat-lng="item" draggable @moveend="log('moveend')">
        <l-popup>
          {{ index + 1 }}
        </l-popup>
      </l-marker>
      <!-- 绘制线段 -->
      <l-polyline :lat-lngs="[
        [47.334852, -1.509485],
        [47.234787, -1.358337, 0],
        [47.342596, -1.328731],
        [47.241487, -1.190568, 1],
      ]" color="red"></l-polyline>

      <!-- 多条线段绘制 -->
      <!-- <l-polyline :lat-lngs="[
    [[45.51, -122.68],
     [37.77, -122.43],
     [34.04, -118.2]],
    [[40.78, -73.91],
     [41.83, -87.62],
     [32.76, -96.72]]
]" color="green"></l-polyline> -->

      <!-- <l-polyline :lat-lngs="[
    [[45.51, -122.68],
     [37.77, -122.43]],
    [[34.04, -118.2],
    [40.78, -73.91]],
     [[41.83, -87.62],
     [32.76, -96.72]]
]" color="green"></l-polyline> -->
      <!-- <l-polyline :lat-lngs="[
    [45.51, -122.68],
     [37.77, -122.43]
]" color="green"></l-polyline>
<l-polyline :lat-lngs="[
    [34.04, -118.2],
    [40.78, -73.91]
]" color="green"></l-polyline> -->

<!-- 多条线段循环绘制 -->
      <div v-for="(item, index) in [
        [[45.51, -122.68],
        [37.77, -122.43]],
        [[34.04, -118.2],
        [40.78, -73.91]],
        [[41.83, -87.62],
        [32.76, -96.72]]
      ]" :key="index">
        <l-polyline class-name="polyline" :lat-lngs="item" color="green"></l-polyline>
      </div>

      <l-polygon :lat-lngs="[
        [46.334852, -1.509485],
        [46.342596, -1.328731],
        [46.241487, -1.190568],
        [46.234787, -1.358337],
      ]" color="#41b782" :fill="true" :fillOpacity="0.5" fillColor="#41b782" />
      <l-rectangle :lat-lngs="[
        [46.334852, -1.509485],
        [46.342596, -1.328731],
        [46.241487, -1.190568],
        [46.234787, -1.358337],
      ]" :fill="true" color="#35495d" />
      <l-rectangle :bounds="[
        [46.334852, -1.190568],
        [46.241487, -1.090357],
      ]">
        <l-popup>
          lol
        </l-popup>
      </l-rectangle>
    </l-map>
    <button @click="changeIcon">New kitten icon</button>
  </div>
</template>
<script>
import { LMap, LTileLayer,LControlLayers,LPopup,LPolygon,LRectangle, LMarker, LTooltip,  LPolyline } from '@vue-leaflet/vue-leaflet'
import "leaflet/dist/leaflet.css";
import RbPoint from './assets/point.png'
import 'leaflet-polylinedecorator'
// import JL from './indexJL'

export default {
  components: {
    LMap,
    // LIcon,
    LTileLayer,
    LMarker,
    LControlLayers,
    LTooltip,
    LPopup,
    LPolyline,
    LPolygon,
    LRectangle,
    // RbPoint
  },
  data() {
    return {
      zoom: 2,
      iconWidth: 25,
      iconHeight: 40,
      line: [],
      drawnItems: '',
      map: ''
    };
  },

  computed: {
    rbPoint() {
      return RbPoint
    },
    iconUrl() {
      return `https://placekitten.com/${this.iconWidth}/${this.iconHeight}`;
    },
    iconSize() {
      return [this.iconWidth, this.iconHeight];
    }
  },
  methods: {
    log(a) {
      console.log(a);
    },
    changeIcon() {
      this.iconWidth += 2;
      if (this.iconWidth > this.iconHeight) {
        this.iconWidth = Math.floor(this.iconHeight / 2);
      }
    },
  }
}
</script>
