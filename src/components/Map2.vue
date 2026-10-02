<script setup>
import 'leaflet/dist/leaflet.css'
import { LMap, LTileLayer, LMarker } from '@vue-leaflet/vue-leaflet'
</script>

<template>
  <div style="height: 90vh">
    <l-map :zoom="8" :center="[38.7, -77.0]">
      <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" />
      <l-marker :lat-lng="[38.7, -77.0]" />
    </l-map>
  </div>
</template>
