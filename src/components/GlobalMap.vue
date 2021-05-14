<template>
  <div style="height: 52vh">
    <LMap
      :zoom="3.5"
      :center="center"
       >
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        layer-type="base"
        name="OpenStreetMap"
      ></l-tile-layer>

      <div v-for="(countryLoc) in countryLoc" :key="countryLoc.country">
      <l-circle-marker
        :lat-lng="countryLoc.countryLoc"
        :radius="5"
      />
      </div>
    </LMap>
  </div>
</template>

<script>
import { LMap, LTileLayer, LCircleMarker } from '@vue-leaflet/vue-leaflet';
import "leaflet/dist/leaflet.css";

export default {
  name: 'GlobalMap',
  props: ['countries', 'countryLoc', 'center'],
  components: {
    LMap,
    LTileLayer,
    LCircleMarker,
  },
  data() {
    console.log(this.countries)
    return {
      zoom: 2,
      radius: 50,
      color: 'green'
    }
  },
}
</script>