<script>
  import { quantize, interpolatePlasma, pie, arc } from 'd3';

  const width = 570;
  const height = width;
  const percent = true;
  const fontSize = 14;
  const strokeWidth = 0;
  const strokeLinejoin = 'round';
  const outerRadius = Math.min(width, height) * 0.5 - 60;
  const innerRadius = 50;
  const labelPosition = 0.6;
  const labelRadius = innerRadius * labelPosition + outerRadius * 0.6;
  const strokeColorWOR = 'white';
  const strokeColorWIR = 'none';
  const stroke = innerRadius > 0 ? strokeColorWIR : strokeColorWOR;
  const padAngle = stroke === 'none' ? 1 / outerRadius : 0;

  export let selectedRegionData = {}; // Initialiser avec un objet vide

  let colorArray = [
    { label: 'Gaz naturel', color: '#FABF64' },
    { label: 'Chaleur fatale - UVE', color: '#E57C33' },
    { label: 'Biomasse', color: '#39AA4A' },
    { label: 'Géothermie', color: '#EC6D6B' },
    { label: 'Chaleur fatale - industrielle', color: '#e0a37b' },
    { label: 'Autres EnR&R', color: '#77BD80' },
    { label: 'Charbon', color: '#808080' },
    { label: 'GOB', color: '#85ad8b' },
    { label: 'Fiouls', color: '#DFDFDF' },
    { label: 'Autres énergies fossiles', color: '#404040' },
  ];

  // Fonction pour transformer les données en tableau d'objets {label, value}
  function transformDataToArray(data) {
    const keysToInclude = [
      'Chaleur fatale - UVE',
      'Chaleur fatale - industrielle',
      'Géothermie',
      'Biomasse',
      'GOB',
      'Autres EnR&R',
      'Gaz naturel',
      'Fiouls',
      'Charbon',
      'Autres énergies fossiles',
    ];

    return keysToInclude
      .map((key) => ({
        label: key,
        value: data && data[key] !== undefined ? data[key] : 0, // Si la clé est absente, utilisez une valeur par défaut de 0
      }))
      .sort((a, b) => b.value - a.value); // Trie par ordre décroissant selon la valeur
  }

  // Initialisation réactive des données `data_mix`
  $: data_mix = selectedRegionData
    ? transformDataToArray(selectedRegionData)
    : [];

  // Calculer les valeurs de `x` et `y` pour les sections du graphique
  $: xVals = data_mix.length > 0 ? data_mix.map((el) => el.label) : [];
  $: yVals = data_mix.length > 0 ? data_mix.map((el) => Number(el.value)) : [];

  $: {
    if (percent && yVals.length > 0) {
      const total = yVals.reduce((a, b) => a + b, 0);
      yVals = total > 0 ? yVals.map((el) => el / total) : yVals; // Éviter de diviser par zéro si `total` est 0
    }
  }

  $: iVals = data_mix.length > 0 ? data_mix.map((el, i) => i) : [];

  // Générer des couleurs pour les sections du graphique
  let colors = colorArray.map((item) => item.color);

  $: {
    if (xVals.length > 0) {
      colors = colorArray.map((item) => item.color);
    }
  }

  // Générer les sections du graphique
  let wedges = [];
  $: {
    if (yVals.length > 0) {
      wedges = pie()
        .padAngle(padAngle)
        .sort(null)
        .value((i) => yVals[i])(iVals);
    }
  }

  $: {
    if (yVals.length > 0) {
      colors = data_mix.map((item) => {
        const colorItem = colorArray.find((c) => c.label === item.label);
        return colorItem ? colorItem.color : '#000000'; // Couleur par défaut si non trouvé
      });
    }
  }

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
  <!-- Tooltip qui s'affiche au survol -->
  <div id="tooltip" class="tooltip"></div>
</div>

<style>
  .chart-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  #mix {
    width: 100%;
    height: 100%;
    position: sticky;
    margin-top: 0;
    transform: translate(0%, 0%);
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
