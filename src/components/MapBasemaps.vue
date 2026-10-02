<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

import markerIcon2x from 'leaflet/dist/images/marker-icon-2x.png'
import markerIcon from 'leaflet/dist/images/marker-icon.png'
import markerShadow from 'leaflet/dist/images/marker-shadow.png'

delete L.Icon.Default.prototype._getIconUrl
L.Icon.Default.mergeOptions({
  iconRetinaUrl: markerIcon2x,
  iconUrl: markerIcon,
  shadowUrl: markerShadow,
})

const mapEl = ref(null)
let map = null

onMounted(() => {
  map = L.map(mapEl.value).setView([38.7, -77.0], 12)

  const osmAttr =
    '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'

  const basemaps = {
    'OSM Standard': L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: osmAttr,
    }),

    'OSM Humanitarian': L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: `${osmAttr}, Tiles: <a href="https://www.hotosm.org/">HOT</a>`,
    }),

    OpenTopoMap: L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
      maxZoom: 17,
      attribution: `${osmAttr}, <a href="https://opentopomap.org">OpenTopoMap</a> (CC-BY-SA)`,
    }),

    'CyclOSM (cycling)': L.tileLayer(
      'https://{s}.tile-cyclosm.openstreetmap.fr/cyclosm/{z}/{x}/{y}.png',
      {
        maxZoom: 20,
        attribution: `${osmAttr}, <a href="https://www.cyclosm.org">CyclOSM</a>`,
      },
    ),

    'Esri Satellite': L.tileLayer(
      'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
      {
        maxZoom: 19,
        attribution: 'Tiles &copy; Esri, Maxar, Earthstar Geographics, and the GIS User Community',
      },
    ),

    'Esri Topo': L.tileLayer(
      'https://server.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer/tile/{z}/{y}/{x}',
      {
        maxZoom: 19,
        attribution: 'Tiles &copy; Esri, USGS, NOAA',
      },
    ),
  }

  basemaps['OSM Humanitarian'].addTo(map)
  L.control.layers(basemaps).addTo(map)
})

onBeforeUnmount(() => {
  map?.remove()
})
</script>

<template>
  <div ref="mapEl" style="height: 500px; width: 100%">
    <l-marker :lat-lng="[38.7, -77.0]" />
  </div>
</template>
