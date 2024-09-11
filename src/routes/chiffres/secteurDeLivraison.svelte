<script>
  import { data } from '$lib/data/data';
  export let currentStep6;
</script>

<div class="divSecteur" style="opacity: {currentStep6 < 1 ? 1 : 0};">
  <div class="grid-container">
    {#each data.secteur_de_livraison as item}
      <div class="grid-item">
        <h3>{item.label}</h3>
        <img
          src={item.icon}
          alt={item.label + ' icon'}
          style="margin-bottom:20px;"
        />
        <p>{item.bar}</p>
      </div>
    {/each}
  </div>
</div>

<style>
  .divSecteur {
    width: 98dvw;
    height: 100dvh;
    position: sticky;
    top: 23%;
    margin-top: 0;
    transform: translate(0%, 0%);
  }

  .grid-container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    text-align: center;
  }

  .grid-item img {
    width: 150px;
    height: auto;
  }

  @media (max-width: 768px) {
    .grid-container {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 480px) {
    .grid-container {
      grid-template-columns: 1fr;
    }
  }
</style>
