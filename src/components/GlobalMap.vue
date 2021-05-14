<template>
  <l-map style="height: 52vh">
     <l-tile-layer
      url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      layer-type="base"
      name="OpenStreetMap"
    ></l-tile-layer>

    <div v-for="(countryInfo,country) in countries" :key="country">
    <l-circle-marker
      :lat-lng="[33, 5]"
      :radius="50"
    />
    </div>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LCircleMarker } from '@vue-leaflet/vue-leaflet';
import "leaflet/dist/leaflet.css";

export default {
  name: 'GlobalMap',
  props: ['countries'],
  components: {
    LMap,
    LTileLayer,
    LCircleMarker,
  },
  data() {
    return {
      zoom: 50,
      radius: 50,
      color: 'green'
    }
  },
}
</script>