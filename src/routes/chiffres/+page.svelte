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
  import DatavizPartDesReseaux from './dataviz_part_des_reseaux.svelte';
  import datavizReseauxChaleurFroid from './dataviz_reseaux_chaleur_froid.svelte';
  import DatavizReseauxChaleurFroid from './dataviz_reseaux_chaleur_froid.svelte';

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

  /**
   * @type {number}
   */
  let currentStep;
  /**
   * @type {number}
   */
  let currentStep2;
  const steps = [
    data.part_des_reseaux_de_chaleur.taux_en_r_r_energie_totale,
    data.part_des_reseaux_de_chaleur.taux_en_r_r_chaleur_totale,
    data.part_des_reseaux_de_chaleur.taux_en_r_r_reseau_de_chaleur,
    '',
  ];

  const steps2 = ['<p>Step 3.</p>'];
</script>

<section>
  <!-- Le graphique en arrière-plan, qui est collé grâce au CSS ci-dessous -->
  <h2
    class="sticky"
    style="position: sticky; top: 12%; z-index: 1000; text-align: center; opacity: {currentStep >=
      steps.length ||
    currentStep == 3 ||
    currentStep == undefined
      ? 0
      : 1};"
  >
    Part des réseaux de chaleur dans la consommation de chaleur
  </h2>
  {#if currentStep != 3}
    <DatavizPartDesReseaux {currentStep} {steps} />
  {/if}
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
  <!-- Le graphique en arrière-plan, qui est collé grâce au CSS ci-dessous -->

  <DatavizReseauxChaleurFroid {currentStep2} />
  <!-- The scrolling container which includes each of the text elements -->
  <Scrolly bind:value={currentStep2}>
    {#each steps as text, i}
      <div class="step" class:active={currentStep2 === i}>
        <div class="step-content">
          {@html text}
        </div>
      </div>
    {/each}
  </Scrolly>
</section>

<style>
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
