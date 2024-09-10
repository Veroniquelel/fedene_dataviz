<script>
  import { data } from '$lib/data/data';
  import { scaleLinear } from 'd3-scale';

  let radiusScale = function (d) {
    return Math.sqrt(d.r * 10); // Utilisation de la racine carrée pour déterminer le rayon
  };

  let width = window.innerWidth * 0.8; // Ajustement de la largeur
  let height = window.innerHeight / 2; // Ajustement de la hauteur

  // Écouteur d'événements pour le redimensionnement de la fenêtre
  window.addEventListener('resize', () => {
    width = window.innerWidth * 0.8;
    height = window.innerHeight * 0.4;
  });

  let xScale = scaleLinear().domain([0, 10]).range([0, width]);
  let yScale = scaleLinear().domain([0, 10]).range([height, 0]);

  export let currentStep; // Recevez la prop ici
  export let steps; // Recevez la prop ici
</script>

<div class="chart" style="opacity: {currentStep !== undefined ? 1 : 0};">
  <svg {width} {height} transform="translate(50%, -50%)">
    <!-- Energie totale -->
    {#each data.part_des_reseaux_de_chaleur.position_valeur_energie_totale as d, index}
      <circle
        cx={width / d.foo}
        cy={height - radiusScale(d) - 40}
        r={radiusScale(d)}
        fill={d.color}
        stroke="#FFFFFF"
        opacity={currentStep === 0 ? 1 : 0.2}
      />
      <foreignObject
        x={width / d.foo - 90}
        y={height - radiusScale(d) - 120}
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
        cy={height - radiusScale(d) - 40}
        r={radiusScale(d)}
        fill={d.color}
        stroke="#FFFFFF"
        opacity={currentStep < 1 ? 0 : currentStep === 1 ? 1 : 0.2}
      />
      <foreignObject
        x={width / d.foo - 90}
        y={height - radiusScale(d) - 90 - 50}
        width="180"
        height="60"
        opacity={currentStep < 1 ? 0 : currentStep === 1 ? 1 : 0}
      >
        <div
          xmlns="http://www.w3.org/1999/xhtml"
          style="font-size: calc(10px + 0.5vw); color: black; text-align: center;"
        >
          {d.chaleur_totale_consommation_finale_totale}
        </div>
      </foreignObject>
    {/each}
    <!-- Ajout de deux ticks X pour l'axe X -->
    <text
      x={width /
        data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[0].foo}
      y={height * 0.95}
      text-anchor="middle"
      class="axis"
      fill="black"
      font-size="calc(12px + 0.5vw)"
    >
      Consommation finale totale
    </text>
    <text
      x={width /
        data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[1].foo}
      y={height * 0.95}
      text-anchor="middle"
      class="axis"
      fill="black"
      font-size="18px"
    >
      Consommation finale en R&R
    </text>
    <!-- Réseau de chaleur totale -->
    {#each data.part_des_reseaux_de_chaleur.position_valeur_reseau_de_chaleur as d, index}
      <circle
        cx={width / d.foo}
        cy={height - radiusScale(d) - 40}
        r={radiusScale(d)}
        fill={d.color}
        stroke="#FFFFFF"
        opacity={currentStep < 1 ? 0 : currentStep >= 2 ? 1 : 0}
      />
      <foreignObject
        x={width / d.foo - 90}
        y={height - radiusScale(d) - 90 - 30}
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
    <!-- Ajout de deux ticks X pour l'axe X -->
    <text
      x={width /
        data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[0].foo}
      y={height * 0.95}
      text-anchor="middle"
      class="axis"
      fill="black"
      font-size="calc(12px + 0.5vw)"
    >
      Consommation finale totale
    </text>
    <text
      x={width /
        data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[1].foo}
      y={height * 0.95}
      text-anchor="middle"
      class="axis"
      fill="black"
      font-size="18px"
    >
      Consommation finale en R&R
    </text>
  </svg>
</div>

<style>
  /* Styles pour rendre les labels text responsives */

  .axis {
    max-width: 100px;
    font-size: calc(
      12px + 0.1vw
    ); /* Ajustement de la taille de la police en fonction de la largeur de l'écran */
    @media (max-width: 768px) {
      font-size: 9px; /* Taille de police fixe pour les petits écrans */
      line-height: 1.5; /* Espacement entre les lignes pour permettre le texte sur 2 lignes */
    }
  }

  /* Le graphique fixe */
  .chart {
    /*    background: whitesmoke; */
    width: 98dvw; /* Largeur responsive */
    height: 100dvh; /* Hauteur responsive */
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2); */
    position: sticky;
    top: 23%;
    margin-top: 0;
    transform: translate(0%, 0%);
  }
</style>
