<script>
  import { PUBLIC_SERVER_BASE_URL } from "$env/static/public";

  let pokemons = $state([]);
  let { selectedPokemon = $bindable() } = $props();
  let selectedGen = $state("all");
  let typedSearch = $state("");
  let filteredPokemons = $derived(
    pokemons.filter(
      (pokemon) =>
        pokemon.name.toLowerCase().includes(typedSearch.toLowerCase()) ||
        pokemon.dexNumber.toString().includes(typedSearch)
    )
  );

  $effect(() => fetchPokemonList(selectedGen));

  async function fetchPokemonList(selectedGen) {
    let response = await fetch(`${PUBLIC_SERVER_BASE_URL}?gen=${selectedGen}`);
    pokemons = await response.json();
  }
</script>

<div id="list-section">
  <div id="search-container">
    <input id="search-bar" type="text" placeholder="Search Pokémon" bind:value={typedSearch} />
    <select bind:value={selectedGen}>
      <option value="all">All generations</option>
      <option value="1">Gen 1</option>
      <option value="2">Gen 2</option>
      <option value="3">Gen 3</option>
      <option value="4">Gen 4</option>
      <option value="5">Gen 5</option>
      <option value="6">Gen 6</option>
      <option value="7">Gen 7</option>
      <option value="8">Gen 8</option>
      <option value="9">Gen 9</option>
    </select>
  </div>
  <div id="pokemon-list">
    {#each filteredPokemons as pokemon}
      <button
        type="button"
        onclick={() => (selectedPokemon = pokemon)}
        class:selected={selectedPokemon?.name === pokemon.name}
      >
        <img
          src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/${pokemon.dexNumber}.png`}
          alt={pokemon.name}
        />
        #{pokemon.dexNumber.toString().padStart(3, "0")}
        {pokemon.name}
      </button>
    {/each}
  </div>
</div>

<style>
  #list-section {
    display: grid;
    grid-template-rows: auto 1fr;
    gap: 10px;
    padding-left: 10px;

    & #search-container {
      display: flex;
    }
  }

  #search-bar {
    flex: 3;
    border: 1px solid transparent;
    border-radius: 10px;
    background-color: #847e89;
    color: white;
    height: 40px;
    padding: 0 10px;
    width: auto;
    margin: 0 3px;
    font: inherit;
    &::placeholder {
      color: white;
    }
  }

  #pokemon-list {
    padding-right: 10px;
    /* Create the scrollbar */
    height: 700px;
    overflow-y: auto; /* add a vertical scrollbar when the content overflows. */
    overflow-x: hidden;

    & button {
      border: 1px solid transparent;
      border-radius: 10px;
      padding: 10px 10px;
      margin: 8px 3px;
      background-color: rgba(23, 35, 60, 0.06);
      transition: background-color 0.2s linear;
      display: flex;
      align-items: center;
      justify-content: left;
      gap: 3px;
      width: 100%;
      font-size: 16px;
      font: inherit;
      color: inherit;
    }

    & .selected {
      background-color: #5c4155c3;
      color: white;
    }

    & img {
      height: 40px;
      width: 40px;
      transition: transform 0.2s ease;
    }

    & button:hover {
      background-color: #918992c0;
      color: white;
      cursor: pointer;
      img {
        transform: scale(1.15);
        filter: drop-shadow(0 0 12px grey);
      }
    }
  }

  select {
    background-color: #56494c;
    color: white;
    border-radius: 8px;
    padding: 8px 12px;
    font-size: 16px;
    font: inherit;
    box-shadow: 3px 3px 0 rgba(0, 0, 0, 0.25);
    cursor: pointer;
    border: none;
    flex: 1;
  }

  #pokemon-list::-webkit-scrollbar {
    width: 15px;
  }

  #pokemon-list::-webkit-scrollbar-track {
    background: #e5e0e1;
    border-radius: 5px;
  }

  #pokemon-list::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 5px;
  }

  @media (max-width: 768px) {
    #pokemon-list {
      height: 350px;
      width: 100%;
    }
  }
</style>
