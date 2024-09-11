<script>
  import { onMount } from 'svelte';
  import maplibre from 'maplibre-gl';

  let map;

  onMount(() => {
    map = new maplibre.Map({
      container: 'map', // ID de l'élément HTML où la carte sera rendue
      style: 'https://demotiles.maplibre.org/style.json', // URL du style de la carte
      center: [2.3522, 48.8566], // Coordonnées de Paris [longitude, latitude]
      zoom: 12, // Niveau de zoom initial
    });
  });
</script>

<div id="map" style="width: 100%; height: 100vh;"></div>

<style>
  #map {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
  }
</style>
