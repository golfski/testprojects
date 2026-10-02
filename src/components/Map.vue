<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

// Fix default marker icons (Vite/Webpack break Leaflet's auto-detected paths)
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

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; OpenStreetMap contributors',
  }).addTo(map)

  L.marker([38.7, -77.0]).addTo(map).bindPopup('Hello from Leaflet!')
})

onBeforeUnmount(() => {
  map?.remove()
})
</script>

<template>
  <div ref="mapEl" class="map"></div>
</template>

<style scoped>
.map {
  height: 400px; /* the container MUST have a height */
  width: 100%;
}
</style>
