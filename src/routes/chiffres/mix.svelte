<script>
  import { quantize, interpolatePlasma, pie, arc } from 'd3';
  import { data } from '$lib/data/data'; // Importer les données

  const width = 570; // the outer width of the chart, in pixels
  const height = width; // the outer height of the chart, in pixels
  const percent = true; // format values as percentages (true/false)
  const fontSize = 14; // the font size of the x and y values
  const strokeWidth = 0; // the width of stroke separating wedges
  const strokeLinejoin = 'round'; // line join style of stroke separating wedges
  const outerRadius = Math.min(width, height) * 0.5 - 60; // the outer radius of the circle, in pixels
  const innerRadius = 50; // the inner radius of the chart, in pixels
  const labelPosition = 0.6; // the position of the label offset from center
  const labelRadius = innerRadius * labelPosition + outerRadius * 0.6; // center radius of labels
  const strokeColorWOR = 'white'; //stroke color when inner radius is greater than 0
  const strokeColorWIR = 'none'; //stroke color when inner radius is 0
  const stroke = innerRadius > 0 ? strokeColorWIR : strokeColorWOR; // stroke separating widths
  const padAngle = stroke === 'none' ? 1 / outerRadius : 0; // angular separation between wedges

  let data_mix = data.data_mix;
  const x = Object.keys(data_mix[0])[0]; // given d in data_mix, returns the (ordinal) x-value
  const y = Object.keys(data_mix[0])[1]; // given d in data_mix, returns the (quantitative) y-value
  const xVals = data_mix.map((el) => el[x]);
  let yVals = data_mix.map((el) => Number(el[y]));
  if (percent) {
    const total = yVals.reduce((a, b) => a + b, 0);
    yVals = yVals.map((el) => el / total);
  }
  const iVals = data_mix.map((el, i) => i);

  // colors can be adjusted manually by creating a color array which length matches length of data_mix set.
  let colors;
  if (!colors)
    colors = quantize((t) => interpolatePlasma(t * 0.7 + 0.3), xVals.length);

  const wedges = pie()
    .padAngle(padAngle)
    .sort(null)
    .value((i) => yVals[i])(iVals);

  const arcPath = arc().innerRadius(innerRadius).outerRadius(outerRadius);

  const arcLabel = arc().innerRadius(labelRadius).outerRadius(labelRadius);

  let tooltip = null;

  // Fonction pour afficher le tooltip
  function showTooltip(event, i) {
    const tooltipElement = document.getElementById('tooltip');
    tooltipElement.innerHTML = `<strong>${xVals[i]}</strong><br>${
      percent
        ? `${(yVals[i] * 100).toFixed(2)}%`
        : yVals[i].toLocaleString('en-US')
    }`;
    tooltipElement.style.display = 'block';
    tooltipElement.style.left = `${event.pageX + 10}px`;
    tooltipElement.style.top = `${event.pageY + 10}px`;
  }

  // Fonction pour masquer le tooltip
  function hideTooltip() {
    const tooltipElement = document.getElementById('tooltip');
    tooltipElement.style.display = 'none';
  }
</script>

<div class="chart-container">
  <svg
    id="mix"
    {width}
    {height}
    viewBox="{-width / 2} {-height / 2} {width} {height}"
  >
    {#each wedges as wedge, i}
      <path
        fill={colors[i]}
        d={arcPath(wedge)}
        {stroke}
        stroke-width={strokeWidth}
        stroke-linejoin={strokeLinejoin}
        on:mouseenter={(event) => showTooltip(event, i)}
        on:mouseleave={hideTooltip}
      />
    {/each}
  </svg>

  <!-- Légende -->
  <div class="legend">
    {#each data_mix as item, i}
      <div class="legend-item">
        <svg width="20" height="20">
          <rect width="20" height="20" fill={colors[i]} />
        </svg>
        <span>{item.label} ({(yVals[i] * 100).toFixed(2)}%) </span>
      </div>
    {/each}
  </div>
</div>

<!-- Tooltip qui s'affiche au survol -->
<div id="tooltip" class="tooltip"></div>

<style>
  .chart-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  #mix {
    width: 98dvw;
    height: 50dvh;
    position: sticky;
    top: 23%;
    margin-top: 0;
    transform: translate(0%, 0%);
    margin-bottom: 120px; /* Ajouter une marge pour séparer la légende du graphique */
  }

  .legend {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }

  .legend-item {
    display: flex;
    align-items: center;
    margin: 5px;
  }

  .legend-item span {
    margin-left: 8px;
    font-size: calc(12px + 0.3vw); /* Taille de police responsive */
  }

  /* Styles pour le tooltip */
  .tooltip {
    position: absolute;
    background-color: white;
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 4px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    display: none;
    pointer-events: none; /* Le tooltip ne bloque pas les interactions de la souris */
  }
</style>
