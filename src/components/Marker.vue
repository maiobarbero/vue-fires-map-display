<template>
  <div>
    <l-marker :lat-lng="markerLatLng">
      <l-tooltip
        >Lat: {{ this.lat }} Long: {{ this.long }}
        <br />
        Happened on {{ this.date }}
      </l-tooltip>
    </l-marker>
  </div>
</template>

<script>
import { LMarker, LTooltip } from "vue2-leaflet";

// resolve missing icon
import { Icon } from "leaflet";
delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  name: "MapMarker",
  components: { LMarker, LTooltip },
  props: ["lat", "long", "date"],

  data() {
    return {
      markerLatLng: [this.lat, this.long],
    };
  },
};
</script>

<style></style>
