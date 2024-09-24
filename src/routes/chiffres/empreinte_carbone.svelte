<script>
  import { onMount } from 'svelte';
  import { scaleLinear } from 'd3-scale';
  import { axisBottom, axisLeft } from 'd3-axis';
  import { select } from 'd3-selection';
  import { area } from 'd3-shape'; // Importer la fonction area
  import { data } from '$lib/data/data';

  export let currentStep5;

  let width = 0; // Initialiser la largeur
  let height = 0; // Initialiser la hauteur
  let empreinte_Carbone = data.empreinte_Carbone;
  let xScale;
  let yScale;
  let areaGenerator;

  // Fonction pour mettre à jour les échelles en fonction de la taille de la fenêtre et des données
  function updateScales() {
    const minX = Math.min(...empreinte_Carbone.map((d) => d.bar)); // Les années deviennent le domaine pour x
    const maxX = Math.max(...empreinte_Carbone.map((d) => d.bar));
    const minY = Math.min(...empreinte_Carbone.map((d) => d.foo)); // Les valeurs de gCO₂ deviennent le domaine pour y
    const maxY = Math.max(...empreinte_Carbone.map((d) => d.foo));

    xScale = scaleLinear()
      .domain([minX - 1, maxX + 1])
      .range([100, width - 100]); // Échelle pour l'axe x (années)

    yScale = scaleLinear()
      .domain([minY - 10, maxY + 10])
      .range([height - 100, 100]); // Échelle pour l'axe y (gCO₂)

    // Générer la fonction d'area
    areaGenerator = area()
      .x((d) => xScale(d.bar))
      .y0(height - 100) // Ligne de base au bas du graphique
      .y1((d) => yScale(d.foo)); // Ligne supérieure basée sur les valeurs y
  }

  updateScales();

  function createAxes() {
    const svg = select('#chartEmpreinte');
    svg.selectAll('.axis').remove(); // Retirer les axes précédents

    // Création de l'axe x (années)
    const xAxis = axisBottom(xScale).tickFormat((d) => `${d}`);
    svg
      .append('g')
      .attr('class', 'axis')
      .attr('transform', `translate(0, ${height - 100})`)
      .call(xAxis);

    // Création de l'axe y (gCO₂)
    const yAxis = axisLeft(yScale).tickFormat((d) => `${d} gCO₂`);
    svg
      .append('g')
      .attr('class', 'axis')
      .attr('transform', `translate(100, 0)`)
      .call(yAxis);
  }

  // Mise à jour des dimensions et des échelles au montage
  onMount(() => {
    if (typeof window !== 'undefined') {
      width = window.innerWidth * 0.8;
      height = window.innerHeight * 0.7;

      updateScales(); // Mise à jour des échelles
      createAxes(); // Création des axes

      const svg = select('#chartEmpreinte');

      // Ajouter la zone avant les cercles
      svg
        .append('path')
        .datum(empreinte_Carbone)
        .attr('fill', '#123828') // Couleur de la zone
        .attr('opacity', '0.3') // Ajouter une opacité de 0.6
        .attr('d', areaGenerator); // Tracer la zone avec les données

      // Ajout des cercles après l'area
      empreinte_Carbone.forEach((d) => {
        svg
          .append('circle')
          .attr('cx', xScale(d.bar))
          .attr('cy', yScale(d.foo))
          .attr('r', 5)
          .attr('fill', 'white')
          .attr('stroke', '#123828')
          .attr('stroke-width', '2');
      });

      // Ajout d'un écouteur pour redimensionner en cas de changement de taille de la fenêtre
      window.addEventListener('resize', () => {
        width = window.innerWidth * 0.8;
        height = window.innerHeight * 0.5;
        updateScales(); // Met à jour les échelles avec les nouvelles dimensions
        createAxes(); // Récréation des axes après redimensionnement

        svg.selectAll('path').remove(); // Supprimer l'ancienne area avant d'ajouter la nouvelle
        svg
          .append('path')
          .datum(empreinte_Carbone)
          .attr('fill', '#123828') // Couleur de la zone
          .attr('opacity', '0.3') // Ajouter une opacité de 0.6
          .attr('d', areaGenerator); // Tracer la zone avec les données

        svg.selectAll('circle').remove(); // Supprimer les cercles avant de les redessiner
        empreinte_Carbone.forEach((d) => {
          svg
            .append('circle')
            .attr('cx', xScale(d.bar))
            .attr('cy', yScale(d.foo))
            .attr('r', 5)
            .attr('fill', 'white')
            .attr('stroke', '#123828')
            .attr('stroke-width', '2');
        });
      });
    }
  });
</script>

<div
  class="chart"
  style="padding: 30px; display: flex; justify-content: center; opacity: {currentStep5 <=
  2
    ? 1
    : 0};"
>
  <svg id="chartEmpreinte" {width} {height}></svg>
</div>

<style>
  .chart {
    width: 98dvw;
    height: 100dvh;
    position: sticky;
    top: 23%;
    margin-top: 0;
    transform: translate(0%, 0%);
  }

  .axis {
    font-size: calc(12px + 0.1vw);
  }

  @media (max-width: 768px) {
    .axis {
      font-size: 9px;
      line-height: 1.5;
    }
  }
</style>
