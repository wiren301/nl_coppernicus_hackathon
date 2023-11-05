<script>
    import { onMount } from 'svelte';
  
    let map;
    let mapContainer;
  
    onMount(() => {
      // Leaflet code should be run here to ensure it's only executed on the client
      const L = require('leaflet');
  
      map = L.map(mapContainer).setView([0, 0], 1);
  
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      }).addTo(map);
    });
  </script>
  
  <svelte:head>
    <!-- Leaflet CSS can still be included in the head as it doesn't rely on the window object -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  </svelte:head>
  
  <style>
    .map {
      height: 100vh;
      width: 100vw;
    }
  </style>
  
  <div class="map" bind:this={mapContainer}></div>
  