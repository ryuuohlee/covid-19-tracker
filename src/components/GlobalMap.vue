<template>
  <div style="height: 52vh">
    <LMap
      :zoom="this.zoom"
      :center="center"
       >
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        layer-type="base"
        name="OpenStreetMap"
      ></l-tile-layer>
      <LMarker :lat-lng="center">

      </LMarker>

      <div v-for="(countryLoc) in countryLoc" :key="countryLoc.country">
        <l-circle-marker
          :lat-lng="countryLoc.countryLoc"
          :color="this.color"
          :fill="true"
          :fillOpacity="0.5"
          fillColor="rgba(6, 95, 70)"
          :radius="Number(countryLoc.radius)"
        >
          <l-tooltip>
            <p>Active: {{numberWithCommas(countryLoc.active)}}</p>
            <p>Infected: {{numberWithCommas(countryLoc.infected)}}</p>
            <p>Recovered: {{numberWithCommas(countryLoc.recovered)}}</p>
            <p>Deaths: {{numberWithCommas(countryLoc.deaths)}}</p>
          </l-tooltip>
        </l-circle-marker>

      </div>
    </LMap>
  </div>
</template>

<script>
import { LMap, LTileLayer, LCircleMarker, LMarker, LTooltip } from '@vue-leaflet/vue-leaflet';
import "leaflet/dist/leaflet.css";

export default {
  name: 'GlobalMap',
  props: ['countries', 'countryLoc', 'center'],
  components: {
    LMap,
    LTileLayer,
    LCircleMarker,
    LMarker,
    LTooltip
  },
  data() {
    return {
      zoom: 3.5,
      color: "rgba(6, 95, 70)"
    }
  },
  methods: {
    numberWithCommas(x) {
        x = x.toString();
        var pattern = /(-?\d+)(\d{3})/;
        while (pattern.test(x))
            x = x.replace(pattern, "$1,$2");
        return x;
    }
  }
}
</script>