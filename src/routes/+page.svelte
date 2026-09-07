<script>
  import Header from "$lib/components/Header.svelte";
  import PokemonDetail from "$lib/components/PokemonDetail.svelte";
  import PokemonList from "$lib/components/PokemonList.svelte";
  import TypeDetail from "$lib/components/TypeDetail.svelte";
  import Footer from "$lib/components/Footer.svelte";
</script>

<div id="container">
  <PokemonList />
  <PokemonDetail />
  <div>
      <TypeDetail />
  </div>
</div>

<style>
  #container {
    display: grid;
    grid-template-columns: 1fr 2fr;
  }
</style>
