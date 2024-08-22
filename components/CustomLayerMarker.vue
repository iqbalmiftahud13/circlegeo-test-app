<template></template>

<script setup>
import { defineProps, watch } from 'vue';
import geojsonData from '~/test.json';
import maplibregl from 'maplibre-gl';

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

    geojsonData.features.forEach((feature) => {
      const coordinates = feature.geometry.coordinates;
      if (Array.isArray(coordinates) && coordinates.length === 2) {
        const [lng, lat] = coordinates; // Destructuring array

        const el = document.createElement('div');
        el.className = 'custom-marker';
        el.style.backgroundImage = 'url(../../../public/circlegeo.jpeg)';
        el.style.width = '50px';
        el.style.height = '50px';
        el.style.backgroundSize = '100%';
        el.style.backgroundSize = 'cover'; 
        el.style.backgroundRepeat = 'no-repeat'; 
        el.style.backgroundPosition = 'center'; 

        new maplibregl.Marker(el)
          .setLngLat([ lng, lat ])
          .addTo(map.value);

        console.log('Marker added at:', [lng, lat]);
      } else {
        console.error('Invalid coordinates format:', coordinates);
      }
    });
  }
});
</script>

<style>
.custom-marker {
  background-size: cover;
}
</style>
