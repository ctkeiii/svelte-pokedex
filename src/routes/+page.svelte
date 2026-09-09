<script>
  import Header from "$lib/components/Header.svelte";
  import PokemonDetail from "$lib/components/PokemonDetail.svelte";
  import PokemonList from "$lib/components/PokemonList.svelte";
  import TypeDetail from "$lib/components/TypeDetail.svelte";
  import Footer from "$lib/components/Footer.svelte";

  let selectedPokemon = $state(null)
</script>

<div id="container">
  <div>
    <PokemonList bind:selectedPokemon/>
  </div>
  <div>
    <PokemonDetail bind:selectedPokemon/>
  </div>
</div>

<style>
  #container {
    display: grid;
    grid-template-columns: 1fr 2fr;
  }
  @media (max-width: 768px) {
    #container {
        grid-template-columns: minmax(0, 1fr);
    }
  }
</style>
