<script>
  import { onMount } from 'svelte';
  import { data } from '$lib/data/data';
  import { scaleLinear } from 'd3-scale';

  let radiusScale = scaleLinear().domain([0, 2000]).range([5, 120]); // Ajustez les valeurs selon vos besoins

  let width = 0; // Initialiser avec une valeur par défaut
  let height = 0; // Initialiser avec une valeur par défaut

  // Cette fonction sera appelée lorsque le composant est monté dans le DOM
  onMount(() => {
    if (typeof window !== 'undefined') {
      width = window.innerWidth * 0.8; // Ajustement de la largeur
      height =
        window.innerWidth < 768
          ? window.innerHeight / 3
          : window.innerHeight / 1.5; // Ajustement de la hauteur selon le type d'appareil

      // Ajout d'un écouteur pour redimensionner en cas de changement de taille de la fenêtre
      window.addEventListener('resize', () => {
        width = window.innerWidth * 0.8;
        height =
          window.innerWidth < 768
            ? window.innerHeight / 3
            : window.innerHeight / 1.5; // Ajustement de la hauteur lors du redimensionnement
      });
    }
  });

  let xScale = scaleLinear().domain([0, 10]).range([0, width]);
  let yScale = scaleLinear().domain([0, 10]).range([height, 0]);

  export let currentStep; // Recevez la prop ici
  export let steps; // Recevez la prop ici

  // Fonction pour calculer le rayon en fonction de la valeur
  function getRadius(d) {
    return radiusScale(d.r); // Utilisez la valeur de d.r pour déterminer le rayon
  }
</script>

<div class="chart" style="opacity: {currentStep !== undefined ? 1 : 0};">
  <svg {width} height={height + 40} viewBox={`0 -20 ${width} ${height + 40}`}>
    <!-- Ajout du padding-bottom -->

    <!-- Energie totale -->
    {#each data.part_des_reseaux_de_chaleur.position_valeur_energie_totale as d, index}
      <circle
        cx={width / d.foo}
        cy={height - getRadius(d) - 40}
        r={getRadius(d)}
        fill={d.color}
        stroke="#FFFFFF"
        opacity={currentStep === 0 ? 1 : 0.2}
      />
      <foreignObject
        x={width / d.foo - 90}
        y={height - getRadius(d) - 120}
        width="180"
        height="60"
        opacity={currentStep === 0 ? 1 : 0}
      >
        <div
          xmlns="http://www.w3.org/1999/xhtml"
          style="font-size: calc(10px + 0.5vw); color: black; text-align: center;"
        >
          {d.energie_total_consommation_finale_totale}
        </div>
      </foreignObject>
    {/each}
    <!-- Chaleur totale -->
    {#each data.part_des_reseaux_de_chaleur.position_valeur_chaleur_totale as d, index}
      <circle
        cx={width / d.foo}
        cy={height - getRadius(d) - 40}
        r={getRadius(d)}
        fill={d.color}
        stroke="#FFFFFF"
        opacity={currentStep < 1 ? 0 : currentStep === 1 ? 1 : 0.2}
      />
      <foreignObject
        x={width / d.foo - 90}
        y={height - getRadius(d) - 130}
        width="180"
        height="60"
        opacity={currentStep < 1 ? 0 : currentStep === 1 ? 1 : 0}
      >
        <div
          xmlns="http://www.w3.org/1999/xhtml"
          style="font-size: calc(10px + 0.5vw); color: black; text-align: center; "
        >
          {d.chaleur_totale_consommation_finale_totale}
        </div>
      </foreignObject>
    {/each}
    <!-- Ajout de deux ticks X pour l'axe X -->
    <foreignObject
      x={width /
        data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[0].foo -
        90}
      y={height * 0.95 - 30}
      width="180"
      height="60"
      style="text-align: -webkit-center;"
    >
      <div
        xmlns="http://www.w3.org/1999/xhtml"
        class="axis"
        style="text-align: center;"
      >
        Consommation finale totale
      </div>
    </foreignObject>
    <foreignObject
      x={width /
        data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[1].foo -
        90}
      y={height * 0.95 - 30}
      width="180"
      height="60"
      style="text-align: -webkit-center;"
    >
      <div
        xmlns="http://www.w3.org/1999/xhtml"
        class="axis"
        style="text-align: center;"
      >
        Consommation finale en R&R
      </div>
    </foreignObject>
    <!-- Réseau de chaleur totale -->
    {#each data.part_des_reseaux_de_chaleur.position_valeur_reseau_de_chaleur as d, index}
      <circle
        cx={width / d.foo}
        cy={height - getRadius(d) - 40}
        r={getRadius(d)}
        fill={d.color}
        stroke="#FFFFFF"
        opacity={currentStep < 1 ? 0 : currentStep >= 2 ? 1 : 0}
      />
      <foreignObject
        x={width / d.foo - 90}
        y={height - getRadius(d) - 120 - (index === 0 ? 30 : 0)}
        width="180"
        height="60"
        opacity={currentStep < 1 ? 0 : currentStep >= 2 ? 1 : 0}
      >
        <div
          xmlns="http://www.w3.org/1999/xhtml"
          style="font-size: calc(10px + 0.5vw); color: black; text-align: center;"
        >
          {d.reseau_de_chaleur_consommation_finale_totale}
        </div>
      </foreignObject>
    {/each}
  </svg>
</div>

<style>
  /* Styles pour rendre les labels text responsives */

  .axis {
    margin-top: 20px;
    max-width: 100%;
    font-size: calc(12px + 0.1vw); /* Ajustement de la taille de la police */
    white-space: normal; /* Autorise le retour à la ligne */
    text-anchor: middle; /* Garde le texte centré sous les éléments */
  }

  @media (max-width: 768px) {
    .axis {
      font-size: 12px; /* Taille de police fixe pour les petits écrans */
      line-height: 1.5; /* Espacement entre les lignes pour permettre le texte sur 2 lignes */
      max-width: 80px; /* Réduis la largeur pour forcer les retours à la ligne */
    }
  }

  /* Le graphique fixe */
  .chart {
    /*    background: whitesmoke; */
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100dvh; /* Hauteur responsive */
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2); */
    position: sticky;
    top: calc(-10%); /* Appliquer -10% en top si on est autre que sur mobile */
    margin-top: 0;
    transform: translate(0%, 0%);
  }

  @media (max-width: 918px) {
    .chart {
      top: 0; /* Réinitialiser à 0 pour les mobiles */
    }
  }

  @media (max-width: 768px) {
    .chart {
      top: 0; /* Réinitialiser à 0 pour les mobiles */
    }
  }
</style>
