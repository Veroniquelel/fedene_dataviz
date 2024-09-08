<script>
  /* Based on the Scrollytelling Starter Kit by
	Connor Rothschild https://twitter.com/CL_Rothschild 
	see his blog post about it: https://www.connorrothschild.com/post/svelte-scrollytelling
	see his REPL: https://svelte.dev/repl/82181dc9c8c04053a7ebabd03c654d1d?version=3.44.3

 This Datasaurus adaption is by: Sebastian Lammers https://vis.social/@seblammers
 */

  import Scrolly from '$lib/components/Scrolly.svelte';

  // extract distinct dataset-names from the data;
  // we'll use those to loop through and trigger
  // the rest of the magic within Scatterplot.svelte;

  let value = 1;
  let distinct = [];
</script>

<section>
  <div class="hero">
    <h1>The Datasaurus Dozen Scrollytelling Example</h1>
    <h2>
      By <a href="https://vis.social/@seblammers" target="_blank">Sebastian</a
      ><br />

      Based on
      <a
        href="https://svelte.dev/repl/82181dc9c8c04053a7ebabd03c654d1d?version=3.44.3"
        target="blank">this REPL</a
      >
      by <a href="https://twitter.com/CL_Rothschild" target="_blank">Connor</a>
    </h2>
  </div>
  <div class="section-container">
    <div class="steps-container">
      <Scrolly bind:value>
        {#each distinct as name, i}
          <div class="step" class:active={value === i}>
            <div class="step-content">
              {i + 1}. This is the "{name}" data configuration.
            </div>
          </div>
        {/each}
        <div class="spacer" />
      </Scrolly>
    </div>
    <div class="sticky"></div>
  </div>
  <div class="hero">
    <h1>Thanks!</h1>
    <p>
      If you want to learn more about this dataset, the Datasaurus Dozen, visit <a
        href="https://www.research.autodesk.com/publications/same-stats-different-graphs/"
        >this site</a
      >.
    </p>
  </div>
</section>

<style>
  :global(body) {
    overflow-x: hidden;
  }

  .hero {
    height: 60vh;
    display: flex;
    place-items: center;
    flex-direction: column;
    justify-content: center;
    text-align: center;
  }

  .hero h2 {
    margin-top: 0;
    font-weight: 200;
  }

  .spacer {
    height: 40vh;
  }

  .sticky {
    position: sticky;
    top: 10%;
    flex: 1 1 60%;
    width: 60%;
  }

  .section-container {
    margin-top: 1em;
    text-align: center;
    transition: background 100ms;
    display: flex;
  }

  .step {
    height: 80vh;
    display: flex;
    place-items: center;
    justify-content: center;
  }

  .step-content {
    font-size: 1rem;
    background: whitesmoke;
    color: #ccc;
    border-radius: 5px;
    padding: 0.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    transition: background 500ms ease;
    box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
    text-align: left;
    width: 75%;
    margin: auto;
    max-width: 500px;
  }

  .step.active .step-content {
    background: white;
    color: black;
  }

  .steps-container,
  .sticky {
    height: 100%;
  }

  .steps-container {
    flex: 1 1 40%;
    z-index: 10;
  }

  /* Comment out the following line to always make it 'text-on-top' */
  @media screen and (max-width: 768px) {
    .section-container {
      flex-direction: column-reverse;
    }
    .sticky {
      width: 95%;
      margin: auto;
    }
  }
</style>
