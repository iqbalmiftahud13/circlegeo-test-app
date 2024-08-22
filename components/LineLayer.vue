<template></template>

<script setup>
import { defineProps, watch } from 'vue';
import geojsonData from '~/test.json';

const props = defineProps({
  map: {
    type: Object,
    required: true
  },
  addLayer: {
    type: Boolean,
    default: false
  }
});

// Watch for changes to the addLayer prop
watch(() => props.addLayer, (newValue) => {
  if (newValue && props.map) {
    const map = props.map;
    
    // Check if the layer exists before adding it
    if (!map.getLayer('LineString')) {
      map.addSource('LineString', {
        type: 'geojson',
        data: geojsonData,
      });

      map.addLayer({
        id: 'LineString',
        type: "line",
        source: "LineString",
        paint: {
          "line-color": ["get", "lineColor"],
          "line-width": 2,
        },
      });

      console.log('LineString layer added');
    }
  }
});
</script>
