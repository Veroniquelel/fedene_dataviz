<script>
  /* Based on the Scrollytelling Starter Kit by
	Connor Rothschild https://twitter.com/CL_Rothschild 
	see his blog post about it: https://www.connorrothschild.com/post/svelte-scrollytelling
	see his REPL: https://svelte.dev/repl/82181dc9c8c04053a7ebabd03c654d1d?version=3.44.3

 This Datasaurus adaption is by: Sebastian Lammers https://vis.social/@seblammers
 */

  import Scrolly from '$lib/components/Scrolly.svelte';
  import { data } from '$lib/data/data';
  import { tweened } from 'svelte/motion';

  import { scaleLinear } from 'd3-scale';
  /* 
  const tweenedX = tweened(
    data.part_des_reseaux_de_chaleur.position_valeur_energie_totale.map(
      (d) => d.foo
    )
  );

  const setFoo = function () {
    tweenedX.set(
      data.part_des_reseaux_de_chaleur.position_valeur_energie_totale.map(
        (d) => d.foo
      )
    );
  };
  const setBar = function () {
    tweenedX.set(
      data.part_des_reseaux_de_chaleur.position_valeur_energie_totale.map(
        (d) => d.foo
      )
    );
  }; */

  let width = window.innerWidth * 0.8; // Ajustement de la largeur
  let height = window.innerHeight * 0.4; // Ajustement de la hauteur

  // Écouteur d'événements pour le redimensionnement de la fenêtre
  window.addEventListener('resize', () => {
    width = window.innerWidth * 0.8;
    height = window.innerHeight * 0.4;
  });

  let xScale = scaleLinear().domain([0, 10]).range([0, width]);
  let yScale = scaleLinear().domain([0, 10]).range([height, 0]);

  /**
   * @type {number}
   */
  let currentStep;
  const steps = [
    data.part_des_reseaux_de_chaleur.taux_en_r_r_energie_totale,
    data.part_des_reseaux_de_chaleur.taux_en_r_r_chaleur_totale,
    data.part_des_reseaux_de_chaleur.taux_en_r_r_reseau_de_chaleur,
    '<p>Step 3.</p>',
  ];

  let radiusScale = function (d) {
    return Math.sqrt(d.r * 10); // Utilisation de la racine carrée pour déterminer le rayon
  };
</script>

<section>
  <!-- Le graphique en arrière-plan, qui est collé grâce au CSS ci-dessous -->
  <h2
    class="sticky"
    style="position: sticky; top: 12%; z-index: 1000; text-align: center;"
  >
    Part des réseaux de chaleur dans la consommation de chaleur
  </h2>
  <div class="chart">
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
          height="50"
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
          data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[0]
            .foo}
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
          data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[1]
            .foo}
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
          data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[0]
            .foo}
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
          data.part_des_reseaux_de_chaleur.position_valeur_energie_totale[1]
            .foo}
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

  <!-- The scrolling container which includes each of the text elements -->
  <Scrolly bind:value={currentStep}>
    {#each steps as text, i}
      <div class="step" class:active={currentStep === i}>
        <div class="step-content">
          {@html text}
        </div>
      </div>
    {/each}
  </Scrolly>
</section>

<style>
  /* The fixed chart */
  /* Le graphique fixe */
  .chart {
    /*    background: whitesmoke; */
    width: 100dvw; /* Largeur responsive */
    height: 100dvh; /* Hauteur responsive */
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2); */
    position: sticky;
    top: 23%;
    margin-top: 0;
    transform: translate(0%, 0%);
  }
  /* Scrollytelling CSS */
  .step {
    height: 80vh;
    display: flex;
    place-items: center;
    justify-content: center;
  }

  .step-content {
    background: whitesmoke;
    color: #ccc;
    border-radius: 5px;
    padding: 0.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    transition: background 500ms ease;
    box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
    z-index: 10;
  }

  .step.active .step-content {
    background: white;
    color: black;
  }

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
</style>
