<script>
  import { onMount } from 'svelte';
  import maplibregl from 'maplibre-gl';
  import { data } from '$lib/data/data';

  /**
   * @type {maplibregl.Map}
   */
  let map;
  let selectedRegionData = null;

  onMount(() => {
    const headerHeight = document.getElementById('header').offsetHeight; // Récupérer la hauteur de l'en-tête
    const mapHeight = window.innerHeight - headerHeight - 40; // Calculer la hauteur de la carte
    document.getElementById('map').style.height = `${mapHeight}px`; // Appliquer la hauteur à la carte

    // les limites pour la France métropolitaine
    const franceBounds = [
      [-6.0, 41.0], // SW
      [11.6, 52.5], // NE
    ];

    map = new maplibregl.Map({
      container: 'map',
      style: 'https://demotiles.maplibre.org/style.json',
      center: [1.8, 47.75],
      zoom: 3,
      maxBounds: franceBounds, // Limiter le panning à la France
    });

    const controlContainer = document.querySelector(
      '.maplibregl-control-container'
    );
    if (controlContainer) {
      controlContainer.style.display = 'none'; // Rendre l'élément invisible
    }

    // Ajouter la source GeoJSON
    map.on('load', () => {
      map.addSource('regions', {
        type: 'geojson',
        data: 'region.geojson',
        generateId: true,
      });

      // Ajouter le layer
      map.addLayer({
        id: 'regions-layer',
        type: 'fill',
        source: 'regions',
        layout: {},
        paint: {
          'fill-color': '#6a9180',
          'fill-opacity': 0.7,
        },
      });

      // Ajouter un contour sur les polygones pour mieux les voir
      map.addLayer({
        id: 'regions-outline',
        type: 'line',
        source: 'regions',
        layout: {},
        paint: {
          'line-color': 'white',
          'line-width': 0.3,
        },
      });
      /* 
      // Ajouter un layer pour les labels (nom des régions)
      map.addLayer({
        id: 'regions-labels',
        type: 'symbol',
        source: 'regions',
        layout: {
          'text-field': ['get', 'nom'], 
          'text-size': 14,
        },
        paint: {
          'text-color': '#000000',
          'text-halo-color': '#ffffff',
          'text-halo-width': 2,
        },
      }); */

      let hoveredRegionId = null;

      // Ajouter l'effet de surbrillance au survol
      map.on('mousemove', 'regions-layer', (e) => {
        map.getCanvas().style.cursor = 'pointer';

        if (e.features.length > 0) {
          if (hoveredRegionId !== null) {
            map.setFeatureState(
              { source: 'regions', id: hoveredRegionId },
              { hover: false }
            );
          }
          hoveredRegionId = e.features[0].id;
          map.setFeatureState(
            { source: 'regions', id: hoveredRegionId },
            { hover: true }
          );
        }
      });

      // Réinitialiser la surbrillance lorsque la souris quitte la région
      map.on('mouseleave', 'regions-layer', () => {
        if (hoveredRegionId !== null) {
          map.setFeatureState(
            { source: 'regions', id: hoveredRegionId },
            { hover: false }
          );
        }
        hoveredRegionId = null;
        map.getCanvas().style.cursor = '';
      });

      // Mettre à jour les styles des polygones sur l'événement hover
      map.addLayer({
        id: 'regions-highlight',
        type: 'fill',
        source: 'regions',
        layout: {},
        paint: {
          'fill-color': '#123828', // Couleur de surbrillance
          'fill-opacity': [
            'case',
            ['boolean', ['feature-state', 'hover'], false],
            0.7,
            0,
          ],
        },
      });
    });

    // Ajouter un événement click sur les régions
    map.on('click', 'regions-layer', (e) => {
      const regionName = e.features[0].properties.nom; // Nom de la région cliquée
      console.log(regionName);
      // Chercher la région dans map_annee2022
      selectedRegionData = data.map_annee2022.find(
        (region) => region['Région'] === regionName
      );
      console.log(selectedRegionData);
      if (selectedRegionData) {
        console.log('Données de la région:', selectedRegionData);

        // Mettre à jour les données de la région cliquée
        data_chaleur = {
          reseaux: selectedRegionData['Nombre réseaux'] || 0,
          chaleur_livree: `${selectedRegionData['Chaleur livrée GWh'] || 0} GWh`,
          taux_EnR_R: `${(selectedRegionData['Taux EnR&R'] * 100).toFixed(1)}%`,
          contenu_CO2: `${selectedRegionData['Contenu CO₂ ACV g/kWh'] || 0} g/kWh`,
          batiments_raccordes:
            selectedRegionData['Nombre bâtiments raccordés'] || 0,
          longueur_reseaux: `${selectedRegionData['Longueur totale'] || 0} km`,
        };

        data_froid = {
          reseaux: selectedRegionData['Nombre réseaux froid'] || 0,
          froid_livre: `${selectedRegionData['Froid livrée MWh'] || 0} MWh`,
          contenu_CO2: `${selectedRegionData['Contenu CO₂ ACV froid g/kWh'] || 0} g/kWh`,
          batiments_raccordes:
            selectedRegionData['Nombre bâtiments raccordés froid'] || 0,
          longueur_reseaux: `${selectedRegionData['Longueur totale froid'] || 0} km`,
        };

        production = {
          chaleur_EnR_R: `${selectedRegionData['Biomasse'] || 0} GWh`, // Peut-être "Biomasse" ou "1ère EnR&R"
          chaleur_industrielle: `${selectedRegionData['Chaleur fatale - industrielle'] || 0} GWh`,
          geothermie: `${selectedRegionData['Géothermie'] || 0} GWh`,
        };
      }
    });

    // Changer le curseur lorsque l'on survole un polygone
    map.on('mouseenter', 'regions-layer', () => {
      map.getCanvas().style.cursor = 'pointer';
    });

    map.on('mouseleave', 'regions-layer', () => {
      map.getCanvas().style.cursor = '';
    });
  });

  let data_chaleur = {
    reseaux: 36,
    chaleur_livree: '803 GWh',
    taux_EnR_R: '76.5%',
    contenu_CO2: '96 g/kWh',
    batiments_raccordes: 1388,
    longueur_reseaux: '227 km',
  };

  let data_froid = {
    reseaux: 0,
    froid_livre: '0 MWh',
    contenu_CO2: '0 g/kWh',
    batiments_raccordes: 0,
    longueur_reseaux: '0 km',
  };

  let production = {
    chaleur_EnR_R: '631 GWh',
    chaleur_industrielle: '132 GWh',
    geothermie: '3.6 GWh',
  };
</script>

<div class="grid-container">
  <!-- Carte avec MapLibre -->
  <div id="map"></div>
  <!-- Colonne de droite avec sous-colonnes -->
  <div class="right-column">
    <!-- Sous-colonne avec 2 rows pour les chiffres clés chaleur et froid -->
    <div>
      <div class="row">
        <div>
          <h3>CHIFFRES CLÉS - chaleur</h3>
          <p>{data_chaleur.reseaux} Réseaux</p>
          <p>{data_chaleur.chaleur_livree} Chaleur livrée</p>
          <p>{data_chaleur.taux_EnR_R} Taux d'EnR&R</p>
          <p>{data_chaleur.contenu_CO2} Contenu CO₂</p>
          <p>{data_chaleur.batiments_raccordes} Bâtiments raccordés</p>
          <p>{data_chaleur.longueur_reseaux} Longueur totale des réseaux</p>
        </div>
      </div>
      <div class="row">
        <div>
          <h3>CHIFFRES CLÉS - froid</h3>
          <p>{data_froid.reseaux} Réseau</p>
          <p>{data_froid.froid_livre} Froid livré</p>
          <p>{data_froid.contenu_CO2} Contenu CO₂</p>
          <p>{data_froid.batiments_raccordes} Bâtiments raccordés</p>
          <p>{data_froid.longueur_reseaux} Longueur totale des réseaux</p>
        </div>
      </div>
    </div>

    <!-- Sous-colonne avec 2 sous-rows pour la production -->
    <div>
      <div class="sub-row">
        <div>
          <h3>Production de chaleur EnR&R</h3>
          <p>{production.chaleur_EnR_R}</p>
        </div>
        <div>
          <h3>Chaleur industrielle</h3>
          <p>{production.chaleur_industrielle}</p>
        </div>
      </div>
      <div class="sub-row">
        <div>
          <h3>Géothermie</h3>
          <p>{production.geothermie}</p>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  /* Rendre la classe invisible */
  .maplibregl-control-container {
    display: none !important; /* Rendre invisible */
  }

  .grid-container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    text-align: left;
    padding: 20px;
  }

  .right-column {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }

  .row {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border: 1px solid #ddd;
    margin-bottom: 10px;
  }

  .sub-row {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border: 1px solid #ddd;
    margin-bottom: 10px;
  }

  #map {
    width: 100%;
    height: 100dvh;
  }

  @media (max-width: 768px) {
    .grid-container {
      grid-template-columns: 1fr;
    }
  }
</style>
