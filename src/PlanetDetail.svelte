<script>
  export let params;
  import { onMount } from "svelte";
  import { push } from "svelte-spa-router";

  let planet = null;

  onMount(async () => {
    const planetId = params.id;
    const res = await fetch(
      `https://api.le-systeme-solaire.net/rest/bodies/${planetId}`,
    );
    const data = await res.json();
    planet = data;
    console.log(planet);
  });

  function goBack() {
    push("/");
  }
</script>

{#if planet}
  <h1>{planet.englishName}</h1>
  <button on:click={goBack}>Zurück</button>
  <ul>
    {#if planet.moons}
      {#each planet.moons as moon}
        <li>{moon.moon}</li>
      {/each}
    {:else}
      <li>No moons</li>
    {/if}
  </ul>
{/if}

<style>
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    margin-bottom: 0.5em;
  }
</style>
