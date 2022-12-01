<script setup lang="ts">
import "mapbox-gl/dist/mapbox-gl.css";
import mapboxgl from "mapbox-gl";
import { onMounted, ref, watch, computed } from "vue";

mapboxgl.accessToken =
  "pk.eyJ1IjoidHVzaGFyLWZ1ZWxidWRkeSIsImEiOiJjbGIzYml6OWswY3EzM3dweDA1am82OGhqIn0.dQ99KMNUXLKu6MXi1VlwxA";

const defaultCoords = {
  lat: 25.198219,
  lng: 55.279792,
};

onMounted(async () => {
  const coordinates: any = document.getElementById("coordinates");
  const map = new mapboxgl.Map({
    container: "map",
    projection: { name: "globe" },
    style: "mapbox://styles/mapbox/streets-v11",
    center: defaultCoords,
    zoom: 10,
    minZoom: 6,
    maxZoom: 15,
  });

  map.addControl(
    new mapboxgl.NavigationControl({
      visualizePitch: true,
    }),
    "bottom-right"
  );

  // gives you your location
  map.addControl(
    new mapboxgl.GeolocateControl({
      positionOptions: {
        enableHighAccuracy: true,
      },
      trackUserLocation: true,
      showUserHeading: true,
    })
  );

  //function to find center coordinates

  // const center: any = computed(() => {
  //   return map.getCenter();
  // });

  // console.log(center.value);

  // marker
  const marker = new mapboxgl.Marker({
    color: "#000000",
    draggable: true,
  })
    .setLngLat([55.279792, 25.198219])
    .addTo(map);

  function onDragEnd() {
    const lngLat: any = marker.getLngLat();

    console.log(lngLat);

    coordinates.style.display = "block";
    coordinates.innerHTML = `Longitude: ${lngLat.lng}<br />Latitude: ${lngLat.lat}`;
  }

  marker.on("dragend", onDragEnd);

  // draggable cursor
  map.on("drag", () => {
    console.log("A dragend event occurred.");
    const point = map.getCenter();
    console.log(point);
    marker.setLngLat(point);
  });
});
</script>

<template>
  <div id="map"></div>
  <pre id="coordinates" class="coordinates"></pre>
  <button>relcoate</button>
</template>

<style scoped></style>
