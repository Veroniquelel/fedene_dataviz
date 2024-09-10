<script>
  import { onMount } from 'svelte';
  import { data } from '$lib/data/data'; // Importer les données
  // @ts-nocheck

  export let currentStep3; // Étape actuelle passée en prop

  let percent = 66.67; // Valeur initiale par défaut
  let donut = true; // Si c'est un donut ou non
  /**
   * @type {any}
   */
  let chart;
  // Fonction pour mettre à jour le donut chart
  function updateDonutChart(el, percent, donut) {
    percent = Math.round(percent);
    if (percent > 100) {
      percent = 100;
    } else if (percent < 0) {
      percent = 0;
    }
    const deg = Math.round(360 * (percent / 100));

    const pie = document.querySelector(el + ' .pie');
    const rightSide = document.querySelector(el + ' .right-side');
    const leftSide = document.querySelector(el + ' .left-side');
    const shadow = document.querySelector(el + ' .shadow');
    const num = document.querySelector(el + ' .num');

    // Vérification si les éléments existent
    if (!pie || !rightSide || !leftSide || !shadow || !num) {
      console.error(
        'Un ou plusieurs éléments sont manquants dans le sélecteur:',
        el
      );
      return; // Sortir de la fonction si un élément est manquant
    }

    if (percent > 50) {
      pie.style.clip = 'rect(auto, auto, auto, auto)';
      rightSide.style.transform = 'rotate(180deg)';
    } else {
      pie.style.clip = 'rect(0, 1em, 1em, 0.5em)';
      rightSide.style.transform = 'rotate(0deg)';
    }

    const borderWidth = donut ? '0.1em' : '0.5em';
    rightSide.style.borderWidth = borderWidth;
    leftSide.style.borderWidth = borderWidth;
    shadow.style.borderWidth = borderWidth;

    num.textContent = percent;
    leftSide.style.transform = `rotate(${deg}deg)`;
  }

  // On met à jour le donut à l'initialisation et en fonction de currentStep3
  onMount(() => {
    const chart = document.querySelector('#specificChart');
    if (chart) {
      updateDonutChart('#specificChart', percent, donut);
    } else {
      console.error("L'élément #specificChart est manquant dans le DOM.");
    }
  });

  // Réaction au changement de currentStep3 pour mettre à jour newPercent
  $: if (
    currentStep3 !== undefined &&
    data.taux_en_R_R[currentStep3] &&
    document.querySelector('#specificChart')
  ) {
    const newPercent = data.taux_en_R_R[currentStep3].chiffre;
    updateDonutChart('#specificChart', newPercent, donut); // Met à jour le donut avec le nouveau pourcentage
  }
</script>

<div id="specificChart" class="donut-size">
  <div class="pie-wrapper">
    <span class="label">
      <span class="num">0</span><span class="smaller">%</span>
    </span>
    <div class="pie">
      <div class="left-side half-circle"></div>
      <div class="right-side half-circle"></div>
    </div>
    <div class="shadow"></div>
  </div>
</div>

<style>
  *,
  *:before,
  *:after {
    box-sizing: border-box;
  }

  #specificChart {
    width: 98dvw;
    height: 100dvh;
    position: sticky;
    top: 23%;
    margin-top: 0;
    transform: translate(0%, 0%);
  }

  /* Styles pour le pourcentage */
  .pie-wrapper {
    position: relative;
    width: 1em;
    height: 1em;
    margin: 0 auto;
  }

  .pie-wrapper .pie {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    clip: rect(0, 1em, 1em, 0.5em);
  }

  .pie-wrapper .half-circle {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: 0.1em solid hsl(168, 76%, 42%);
    border-radius: 50%;
    clip: rect(0em, 0.5em, 1em, 0em);
  }

  .pie-wrapper .right-side {
    transform: rotate(0deg);
  }

  .pie-wrapper .label {
    position: absolute;
    top: 0.52em;
    right: 0.4em;
    bottom: 0.4em;
    left: 0.4em;
    display: block;
    background: none;
    border-radius: 50%;
    color: #7f8c8d;
    font-size: 0.25em;
    line-height: 2.6em;
    text-align: center;
    cursor: default;
    z-index: 2;
  }

  .pie-wrapper .smaller {
    padding-bottom: 20px;
    color: #bdc3c7;
    font-size: 0.45em;
    vertical-align: super;
  }

  .pie-wrapper .shadow {
    width: 100%;
    height: 100%;
    border: 0.1em solid #bdc3c7;
    border-radius: 50%;
  }

  .donut-size {
    font-size: 12em;
  }
</style>
