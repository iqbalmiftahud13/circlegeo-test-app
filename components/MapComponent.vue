<template>
  <div ref="mapContainer" class="map-container">
    <button @click="toggleLayerLine">Line Layer</button>
    <button @click="toggleLayerPolygon">Polygon Layer</button>
    <button @click="toggleLayerCircle">Circle Layer</button>
    <button @click="toggleLayerCustomMaker">Custom Marker</button>
    <button @click="toggleLayerMaker">Marker</button>


    <LineLayer :map="map" :addLayer="showLineLayer" />
    <PolygonLayer :map="map" :addLayer="showPolygonLayer" />
    <CircleLayer :map="map" :addLayer="showCircleLayer" />
    <CustomLayerMarker :map="map" :addLayer="showCustomMarker" />
    <DefaultLayerMarker :map="map" v-if="showMarker"/>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import maplibregl from 'maplibre-gl';
import geojsonData from '~/test.json';
import LineLayer from './LineLayer.vue';
import PolygonLayer from './PolygonLayer.vue';
import CircleLayer from './CircleLayer.vue';
import CustomLayerMarker from './CustomLayerMarker.vue';
import DefaultLayerMarker from './DefaultLayerMarker.vue';

const mapContainer = ref(null);
const map = ref(null);
const showLineLayer = ref(false);
const showPolygonLayer = ref(false);
const showCircleLayer = ref(false);
const showCustomMarker = ref(false);
const showMarker = ref(false);

const toggleLayerLine = () => {
  showLineLayer.value = !showLineLayer.value;
};
const toggleLayerPolygon = () => {
  showPolygonLayer.value = !showPolygonLayer.value;
};
const toggleLayerCircle = () => {
  showCircleLayer.value = !showCircleLayer.value;
};
const toggleLayerCustomMaker = () => {
  showCustomMarker.value = !showCustomMarker.value;
};
const toggleLayerMaker = () => {
  showMarker.value = !showMarker.value;
};

// const addCustomMarkers = () => {
//   if (map.value) {
//     geojsonData.features.forEach((feature) => {
//       const coordinates = feature.geometry.coordinates;
//       if (Array.isArray(coordinates) && coordinates.length === 2) {
//         const [lng, lat] = coordinates; // Destructuring array

//         const el = document.createElement('div');
//         el.className = 'custom-marker';
//         el.style.backgroundImage = 'url(../../../public/circlegeo.jpeg)';
//         el.style.width = '50px';
//         el.style.height = '50px';
//         el.style.backgroundSize = '100%';
//         el.style.backgroundSize = 'cover'; 
//         el.style.backgroundRepeat = 'no-repeat'; 
//         el.style.backgroundPosition = 'center'; 

//         new maplibregl.Marker(el)
//           .setLngLat([ lng, lat ])
//           .addTo(map.value);

//         console.log('Marker added at:', [lng, lat]);
//       } else {
//         console.error('Invalid coordinates format:', coordinates);
//       }
//     });
//   } else {
//     console.error('Map is not initialized.');
//   }
// };
onMounted(() => {
  map.value = new maplibregl.Map({
    container: mapContainer.value,
    style: 'https://demotiles.maplibre.org/style.json',
    center: [113.85, -0.4],
    zoom: 4.11,
  });

  map.value.on('load', () => {
    console.log('Map loaded');
  });
});
</script>

<style>
.map-container {
  width: 100%;
  height: 100vh;
}
.custom-marker {
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
</style>
