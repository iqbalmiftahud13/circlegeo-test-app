<template></template>

<script setup>
import { onMounted } from "vue";
import { map } from "./MapComponent.vue";

onMounted(() => {
  map.on("load", () => {
    map.addSource("vectorTileLayer", {
      type: "raster",
      tiles: [
        "https://rami.bmkg.go.id/api/windtemp_get/wafc/WT/snow/850/202405121800/202405131200/{z}/{x}/{y}.png",
      ],
      tileSize: 256,
    });

    map.addLayer({
      id: "vectorTileLayer",
      type: "raster",
      source: "vectorTileLayer",
      paint: {
        "raster-opacity": 0.85,
      },
    });
  });
});
</script>
