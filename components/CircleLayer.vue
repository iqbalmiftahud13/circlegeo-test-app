<template></template>

<script setup>
import { defineProps, watch } from "vue";
import geojsonData from "~/test.json";

const props = defineProps({
  map: {
    type: Object,
    required: true,
  },
  addLayer: {
    type: Boolean,
    default: false,
  },
});

// Watch for changes to the addLayer prop
watch(
  () => props.addLayer,
  (newValue) => {
    if (newValue && props.map) {
      const map = props.map;

      // Check if the layer exists before adding it
      if (!map.getLayer("CircleLayer")) {
        map.addSource("CircleLayer", {
          type: "geojson",
          data: geojsonData,
        });

        map.addLayer({
          id: "CircleLayer",
          type: "circle",
          source: "CircleLayer",
          paint: {
            "circle-color": "#ff0000", 
            "circle-radius": 10, 
            "circle-opacity": 0.5,
          },
        });

        console.log("CircleLayer layer added");
      }
    }
  }
);
</script>
