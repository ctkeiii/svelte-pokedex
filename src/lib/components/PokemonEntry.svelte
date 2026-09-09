<script>
  let { pokemonDetails } = $props();
</script>

{#if !pokemonDetails}
  <div id="pokemon-entry" class="placeholder">
    <p>Pokédex entry</p>
    <p>???????????????????????</p>
  </div>
{:else}
  <div id="pokemon-entry">
    <p>Pokédex entry</p>
    <p>{pokemonDetails.dexEntry}</p>
  </div>
{/if}

<style>
  #pokemon-entry {
    background-color: rgb(211, 204, 204);
    border: 1px solid transparent;
    border-radius: 10px;
    padding: 15px;
    margin-bottom: 10px;
    color: rgb(73, 67, 67);
    & p:nth-child(1) {
      font-family: pokemon;
      font-size: 30px;
      margin: 0 0 10px 0;
      filter: drop-shadow(2px 2px 0 rgba(0, 0, 0, 0.2));
    }
    & p:nth-child(2) {
      margin: 0;
    }
  }
  .placeholder{
    opacity: 0.5;
  }
</style>
