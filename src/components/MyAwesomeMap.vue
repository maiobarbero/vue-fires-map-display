<template>
  <div>
    <div class="map-title">
      <h3>NASA Fires Stats</h3>
      <p>First, pick up a date</p>

      <select v-model="currentFilter" class="selection">
        <option disabled value="">Please select a day</option>
        <option value="all">Show All</option>
        <option v-for="(fire, index) in filter" :key="index">
          {{ fire.date }}
        </option>
      </select>
    </div>

    <div class="map">
      <l-map
        style="height: 80%; width: 100%"
        :zoom="zoom"
        :center="center"
        @update:zoom="zoomUpdated"
        @update:center="centerUpdated"
      >
        <l-tile-layer :url="url"></l-tile-layer>
        <MapMarker
          v-for="(fire, index) in filteredFires"
          :key="index"
          :lat="fire.latitude"
          :long="fire.longitude"
          :date="fire.date"
        />
      </l-map>
    </div>
  </div>
</template>

<script>
import { LMap, LTileLayer } from "vue2-leaflet";
import MapMarker from "./Marker";

export default {
  name: "MyAwesomeMap",
  components: {
    LMap,
    LTileLayer,
    MapMarker,
  },
  props: ["fires"],

  data() {
    return {
      currentFilter: "",
      filteredFires: [],
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      zoom: 2,
      center: [47.41322, -1.219482],
      bounds: null,
    };
  },

  methods: {
    zoomUpdated(zoom) {
      this.zoom = zoom;
    },
    centerUpdated(center) {
      this.center = center;
    },
    filterByDate() {
      this.filteredFires = [];
      if (this.currentFilter == "all") {
        this.filteredFires = this.filteredFires.concat(this.fires);
      } else {
        this.filteredFires = this.fires.filter(
          (fire) => fire.date == this.currentFilter
        );
      }
    },
  },

  watch: {
    currentFilter() {
      this.filterByDate();
    },
  },
  computed: {
    filter() {
      return [...new Map(this.fires.map((fire) => [fire.date, fire])).values()];
    },
  },
};
</script>
<style>
.selection {
  height: 40px;
}
.map {
  height: 550px;
  width: 50%;
  margin: 0 auto;
}
.map-title {
  display: flex;
  align-items: center;
  justify-content: space-around;
  width: 100%;
  height: 90px;
  margin: 0 auto;
  border-bottom: 2px solid #fefefe;
  margin-bottom: 50px;
}

.map-title h3 {
  font-size: 32px;
}
.map-title p {
  font-size: 22px;
}
</style>
