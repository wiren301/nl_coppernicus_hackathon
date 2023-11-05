<script>
    import { getContext, onMount } from "svelte";
    import L from 'leaflet';
  
    export let lat;
    export let lng;
  
    let marker;
  
    onMount(() => {
      // Ensure that the 'leafletMapInstance' context has been provided by a parent component
      const map = getContext('leafletMapInstance');
      if (map) {
        marker = L.marker([lat, lng]).addTo(map);
      } else {
        console.error('leafletMapInstance context has not been set.');
      }
    });
  
    // Optional: Cleanup if the component is destroyed
    onDestroy(() => {
      if (marker) {
        marker.remove();
      }
    });
  </script>
  