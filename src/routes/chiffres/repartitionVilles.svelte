<script>
  let repartition_villes = [
    { label: '2000 réseaux petites villes', value: 2000, color: '#f4a261' },
    { label: '150 réseaux grandes villes', value: 150, color: '#702e1e' },
  ];

  let maxValue = Math.max(...repartition_villes.map((item) => item.value));
</script>

<div class="rectVille">
  <div class="grid-container">
    {#each repartition_villes as item, index}
      <div class="grid-item">
        <div
          class="bar"
          style="background-color: {item.color}; height: {(item.value /
            maxValue) *
            300}px; margin-bottom:20px"
        ></div>
        <p>{item.label}</p>
      </div>
    {/each}
  </div>
</div>

<style>
  .rectVille {
    width: 98dvw;
    height: 100dvh;
    position: sticky;
    top: 23%;
    margin-top: 0;
    transform: translate(0%, 0%);
  }

  .grid-container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
    text-align: center;
    align-items: end;
  }

  .grid-item {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .bar {
    width: 100px;
    background-color: #f4a261; /* Couleur de base */
    transition: height 0.3s ease;
  }

  .bar:nth-child(2) {
    background-color: #702e1e; /* Couleur différente pour la 2e barre */
  }

  @media (max-width: 768px) {
    .grid-container {
      grid-template-columns: 1fr;
    }
  }
</style>
