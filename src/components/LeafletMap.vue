<template>
  <div id="map"></div>
</template>

<script setup>
import L from "leaflet";
import "leaflet/dist/leaflet.css";
import "leaflet.markercluster/dist/MarkerCluster.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import { onMounted } from "vue";
import "leaflet.markercluster";

onMounted(loadMap);

function loadMap() {
  // Asuncion - Paraguay
  const longitud = -57.6309129;
  const latitud = -25.2961407;

  const zoom = 10;
  const minZoom = 6;
  const maxZoom = 18;

  let map = new L.map("map", {
    center: [latitud, longitud],
    zoom: zoom,
    minZoom: minZoom,
    maxZoom: maxZoom,
  });

  L.tileLayer("https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png", {
    maxZoom: 18,
    attribution:
      'Data \u00a9 <a href="http://www.openstreetmap.org/copyright">' +
      "OpenStreetMap Contributors </a> Tiles \u00a9 HOT",
  }).addTo(map);

  fetch("/src/data/cajeros.geojson")
    .then(function (response) {
      return response.json();
    })
    .then(function (data) {
      const markers = L.geoJSON(data);
      const markerCluster = L.markerClusterGroup();
      markerCluster.addLayer(markers);
      map.addLayer(markerCluster);
    });
}
</script>

<style scoped>
#map {
  width: 100%;
  height: 500px;
}
</style>
