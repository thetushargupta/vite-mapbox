<script setup lang="ts">
import { onMounted } from "vue";
import "maplibre-gl/dist/maplibre-gl.css";
import maplibregl from "maplibre-gl";

const defaultCoords = {
  lat: 25.198219,
  lng: 55.279792,
};

onMounted(async () => {
  const map2 = new maplibregl.Map({
    container: "map2",
    style:
      "https://api.maptiler.com/maps/streets-v2/style.json?key=Wsytqqzjlw8eejm6lW3O",
    center: defaultCoords,
    zoom: 10,
    minZoom: 6,
    maxZoom: 15,
  });

  map2.addControl(
    new maplibregl.NavigationControl({
      visualizePitch: true,
    }),
    "bottom-right"
  );

  // gives you your location
  map2.addControl(
    new maplibregl.GeolocateControl({
      positionOptions: {
        enableHighAccuracy: true,
      },
      trackUserLocation: true,
    })
  );

  // marker
  const marker = new maplibregl.Marker({
    color: "#000000",
    draggable: true,
  })
    .setLngLat([55.279792, 25.198219])
    .addTo(map2);

  // dynamic marker location function
  map2.on("drag", () => {
    console.log("A dragend event occurred.");
    const point = map2.getCenter();
    console.log(point);
    marker.setLngLat(point);
  });
});
</script>

<template>
  <div id="map2"></div>
</template>

<style scoped></style>
