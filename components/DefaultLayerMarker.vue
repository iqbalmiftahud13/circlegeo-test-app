<template></template>

<script setup>
import { defineProps, watch } from 'vue';
import maplibregl from 'maplibre-gl';
import geojsonData from '~/test.json';

const props = defineProps({
  map: {
    type: Object,
    required: true
  }
});

// Watch for changes to the map prop or any other relevant data
watch(() => props.map, (newMap) => {
  if (newMap) {
    newMap.on('load', () => {
      geojsonData.features.forEach((feature) => {
        const coordinates = feature.geometry.coordinates;
        if (Array.isArray(coordinates) && coordinates.length === 2) {
          const [lng, lat] = coordinates;

          new maplibregl.Marker()
            .setLngLat([lng, lat])
            .addTo(newMap);

          console.log('Marker added at:', [lng, lat]);
        } else {
          console.error('Invalid coordinates format:', coordinates);
        }
      });
    });
  }
},
{ immediate: true });
</script>
