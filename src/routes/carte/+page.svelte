<script>
  // @ts-nocheck

  import { onMount } from 'svelte';
  import maplibregl from 'maplibre-gl';
  import { data } from '$lib/data/data';
  import MixChart from './mix.svelte';

  /**
   * @type {maplibregl.Map}
   */
  let map;
  let selectedRegionData = null;

  // @ts-ignore
  let region = 'Bretagne';

  onMount(() => {
    /*         document.getElementById('map').style.height = `${mapHeight}px`; // Appliquer la hauteur à la carte
    document.getElementById('number').style.height = `${mapHeight}px`; // Appliquer la hauteur à la carte
 */

    // @ts-ignore
    const headerHeight = document.getElementById('header').offsetHeight; // Récupérer la hauteur de l'en-tête
    const nameRegionHeight = document.querySelector('.nameRegion').offsetHeight; // Récupérer la hauteur de l'élément nameRegion

    const mapHeight = window.innerHeight - headerHeight - nameRegionHeight - 40; // Calculer la hauteur de la carte
    const contHeight =
      window.innerHeight - headerHeight - nameRegionHeight - 100; // Calculer la hauteur de la carte
    // @ts-ignore
    document.getElementById('grid').style.height = `${contHeight}px`; // Appliquer la hauteur à la carte */
    document.getElementById('map').style.height = `${mapHeight}px`; // Appliquer la hauteur à la carte */
    document.getElementById('number').style.height = `${mapHeight}px`; // Appliquer la hauteur à la carte */
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

      /**
       * @type {string | number | null | undefined}
       */
      let hoveredRegionId = null;

      // Ajouter l'effet de surbrillance au survol
      map.on('mousemove', 'regions-layer', (e) => {
        map.getCanvas().style.cursor = 'pointer';

        // @ts-ignore
        if (e.features.length > 0) {
          if (hoveredRegionId !== null) {
            map.setFeatureState(
              { source: 'regions', id: hoveredRegionId },
              { hover: false }
            );
          }
          // @ts-ignore
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
      // @ts-ignore
      const regionName = e.features[0].properties.nom; // Nom de la région cliquée
      console.log(regionName);
      region = regionName;
      // Chercher la région dans map_annee2022
      selectedRegionData = data.map_annee2022.find(
        (region) => region['Région'] === regionName
      );
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
          bois: `${selectedRegionData['1ère EnR&R'] || 0} GWh`, // Peut-être "Biomasse" ou "1ère EnR&R"
          recycle: `${selectedRegionData['2e EnR&R'] || 0} GWh`,
          hydro: `${selectedRegionData['3e EnR&R'] || 0} GWh`,
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

  // Chercher la région dans map_annee2022
  selectedRegionData = data.map_annee2022.find(
    (region) => region['Région'] === 'Bretagne'
  );

  // Mettre à jour les données de la région cliquée
  let data_chaleur = {
    reseaux: selectedRegionData['Nombre réseaux'] || 0,
    chaleur_livree: `${selectedRegionData['Chaleur livrée GWh'] || 0} GWh`,
    taux_EnR_R: `${(selectedRegionData['Taux EnR&R'] * 100).toFixed(1)}%`,
    contenu_CO2: `${selectedRegionData['Contenu CO₂ ACV g/kWh'] || 0} g/kWh`,
    batiments_raccordes: selectedRegionData['Nombre bâtiments raccordés'] || 0,
    longueur_reseaux: `${selectedRegionData['Longueur totale'] || 0} km`,
  };

  let data_froid = {
    reseaux: selectedRegionData['Nombre réseaux froid'] || 0,
    froid_livre: `${selectedRegionData['Froid livrée MWh'] || 0} MWh`,
    contenu_CO2: `${selectedRegionData['Contenu CO₂ ACV froid g/kWh'] || 0} g/kWh`,
    batiments_raccordes:
      selectedRegionData['Nombre bâtiments raccordés froid'] || 0,
    longueur_reseaux: `${selectedRegionData['Longueur totale froid'] || 0} km`,
  };

  let production = {
    bois: `${selectedRegionData['1ère EnR&R'] || 0} GWh`, // Peut-être "Biomasse" ou "1ère EnR&R"
    recycle: `${selectedRegionData['2e EnR&R'] || 0} GWh`,
    hydro: `${selectedRegionData['3e EnR&R'] || 0} GWh`,
  };
</script>

<div id="grid" class="grid-container">
  <!-- Carte avec MapLibre -->
  <h2 class="nameRegion" align="center">{region}</h2>
  <div>
    <div id="map"></div>
  </div>
  <!-- Colonne de droite avec sous-colonnes -->

  <!-- Deuxième colonne -->
  <div id="number" class="right-column">
    <!-- Première row avec deux colonnes : chiffres chaud/froid et mix énergétique -->
    <div class="row">
      <!-- Colonne pour chiffres chauds et froids -->
      <div class="key-figures-column">
        <div class="gridChaud">
          <h4 class="chaud" align="center">CHIFFRES CLÉS - chaleur</h4>
          <p>
            <span class="highlightChaud">{data_chaleur.reseaux}</span> Réseaux
          </p>
          <p>
            <span class="highlightChaud">{data_chaleur.chaleur_livree}</span> Chaleur
            livrée
          </p>
          <p>
            <span class="highlightChaud">{data_chaleur.taux_EnR_R}</span> Taux d'EnR&R
          </p>
          <p>
            <span class="highlightChaud">{data_chaleur.contenu_CO2}</span> Contenu
            CO₂
          </p>
          <p>
            <span class="highlightChaud"
              >{data_chaleur.batiments_raccordes}</span
            > Bâtiments raccordés
          </p>
          <p>
            <span class="highlightChaud">{data_chaleur.longueur_reseaux}</span> Longueur
            totale des réseaux
          </p>
        </div>
        <div class="gridFroid">
          <h4 class="froid" align="center">CHIFFRES CLÉS - froid</h4>
          <p>
            <span class="highlightFroid">{data_froid.reseaux}</span> Réseaux
          </p>
          <p>
            <span class="highlightFroid">{data_froid.froid_livre}</span> Froid livrée
          </p>
          <p>
            <span class="highlightFroid">{data_froid.contenu_CO2}</span> Contenu
            CO₂
          </p>
          <p>
            <span class="highlightFroid">{data_froid.batiments_raccordes}</span>
            Bâtiments raccordés
          </p>
          <p>
            <span class="highlightFroid">{data_froid.longueur_reseaux}</span> Longueur
            totale des réseaux
          </p>
        </div>
      </div>

      <!-- Colonne pour le mix énergétique -->
      <div class="mix-energetique">
        <h4 class="chaud2" align="center">Mix énergétique en production</h4>
        <MixChart {selectedRegionData} />
      </div>
    </div>

    <!-- Deuxième row pour la production de chaleur EnR&R -->
    <div class="row production-row">
      <!-- Première sous-row pour le titre -->
      <div class="subRowProd">
        <div class="row-title">
          <h4 class="chaud2" align="center">Production de chaleur EnR&R</h4>
        </div>

        <!-- Deuxième sous-row pour le contenu -->
        <div class="row-content">
          <div class="item">
            <img src="wood.svg" alt="Logo 1" class="logo2" />
            <p>{production.bois}</p>
          </div>
          <div class="item">
            <img src="bin.svg" alt="Logo 2" class="logo2" />
            <p>{production.recycle}</p>
          </div>
          <div class="item">
            <img src="hydro.svg" alt="Logo 3" class="logo2" />
            <p>{production.hydro}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  p {
    font-size: 1rem;
  }

  /* Taille de police ajustée pour les écrans de tablette */
  @media (max-width: 768px) {
    p {
      font-size: 0.8rem;
    }
  }

  .other {
    border: 2px solid #e75353;
  }
  /* Rendre la classe invisible */
  .maplibregl-control-container {
    display: none !important; /* Rendre invisible */
  }

  .highlightChaud {
    color: #e75353; /* Couleur des données */
    font-weight: bold; /* Texte en gras */
  }

  .highlightFroid {
    color: #4aa6e0; /* Couleur des données */
    font-weight: bold; /* Texte en gras */
  }

  .nameRegion {
    grid-column: 1 / -1; /* Occupe toute la largeur */
    margin: 0;
  }

  .subRowProd {
    border: 2px solid #e75353;
    padding: 10px;
  }

  .grid-container {
    display: grid;
    grid-template-columns: 45% 54%; /* La carte prend 45% et la colonne droite 55% */
    gap: 10px;
    text-align: left;
    padding: 20px;
  }

  .chaud {
    background-color: #fe000028;
    color: #d52121;
    font-weight: 600;
  }

  .chaud2 {
    color: #d52121;
    font-weight: 600;
  }

  .gridChaud {
    color: #d52121;
    padding: 5px;
    border: 2px solid #e75353;
    --bs-gutter-x: 0;
  }

  .froid {
    background-color: #0094fe28;
    color: #4aa6e0;
    font-weight: 600;
  }

  .gridFroid {
    color: #4aa6e0;
    padding: 5px;
    border: 2px solid #4aa6e0;
    width: 100%; /* Assurez que .gridFroid prend toute la largeur */
    box-sizing: border-box; /* Pour inclure le padding et la bordure dans la largeur */
    --bs-gutter-x: 0;
  }

  .right-column {
    display: grid;
    /*    grid-template-columns: 1fr 1fr; */
    gap: 10px;
  }

  /* Première row avec deux colonnes */
  .row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    /*  gap: 20px; */
    color: black !important;
    font-weight: 500;
  }

  /* Colonne pour chiffres clés (chaud et froid) */
  .key-figures-column {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  /* Grid pour chiffres chaud et froid */
  .gridChaud,
  .gridFroid {
    padding: 10px;
    border: 2px solid;
    text-align: center;
  }

  .gridChaud {
    border-color: #e75353;
  }

  .gridFroid {
    border-color: #4aa6e0;
    height: 100%;
  }

  /* Colonne pour le mix énergétique */
  .mix-energetique {
    padding: 10px;
    border: 2px solid #e75353;
    text-align: center;
  }

  .sub-row {
    display: flex;
    justify-content: space-between;
    padding: 10px;
  }

  #map {
    width: auto;
    height: 100%;
  }

  @media (max-width: 768px) {
    .grid-container {
      grid-template-columns: 1fr;
    }
  }

  /* Conteneur général pour la production */
  .production-row {
    display: flex;
    flex-direction: column;
    padding-left: 12px;
    gap: 20px;
  }

  /* Première sous-row pour le titre */
  .row-title {
    text-align: center;
  }

  /* Deuxième sous-row pour le contenu */
  .row-content {
    display: flex;
    justify-content: space-between;
    gap: 20px;
    text-align: center;
  }

  /* Style pour les items individuels (bois, recycle, hydro) */
  .item {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* Style pour les images */
  .logo2 {
    width: 50px;
    height: 50px;
  }
</style>
