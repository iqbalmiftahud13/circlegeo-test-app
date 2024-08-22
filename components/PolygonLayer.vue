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
    if (!map.getLayer('Polygon')) {
      map.addSource('Polygon', {
        type: 'geojson',
        data: geojsonData,
      });

      map.addLayer({
        id: 'Polygon',
        type: 'fill',
        source: 'Polygon',
        paint: {
          'fill-color': ['get', 'fillColor'],
          'fill-opacity': 0.5,
          'fill-outline-color': ['get', 'lineColor'],
        },
      });

      console.log('Polygon layer added');
    }
  }
});
</script>
