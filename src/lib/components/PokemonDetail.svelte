<script>
  import { PUBLIC_SERVER_BASE_URL } from "$env/static/public";
  import { PUBLIC_POKEMON_DETAIL_URL } from "$env/static/public";
  import { PUBLIC_POKEMON_EVOLUTION_URL } from "$env/static/public";
  import TypeDetail from "./TypeDetail.svelte";
  import PokemonEntry from "./PokemonEntry.svelte";

  let { selectedPokemon } = $props();
  let pokemonDetails = $state(null);
  let cardDetails = $state(null);
  let speciesDetails = $state(null);
  let evolutionChain = $state(null);
  let imageLoaded = $state(false);
  let isShake = $state(false);

  $effect(() => {
    if (selectedPokemon) {
      cardDetails = null; // for loading...
      speciesDetails = null;
      evolutionChain = null;
      imageLoaded = false;
      pokemonDetails = null;

      fetchPokemonDetails();
      fetchCardDetails();
    }
  });

  async function fetchPokemonDetails() {
    let response = await fetch(`${PUBLIC_SERVER_BASE_URL}/${selectedPokemon.dexNumber}`);
    pokemonDetails = await response.json();
  }

  async function fetchCardDetails() {
    let response = await fetch(`${PUBLIC_POKEMON_DETAIL_URL}/${selectedPokemon.dexNumber}`);
    cardDetails = await response.json();
    let response2 = await fetch(
      `${PUBLIC_POKEMON_DETAIL_URL}-species/${selectedPokemon.dexNumber}`
    );
    speciesDetails = await response2.json();
    fetchEvolutionChain();
  }

  async function fetchEvolutionChain() {
    let response = await fetch(speciesDetails?.evolution_chain.url);
    evolutionChain = await response.json();
  }

  function convertHeight(height) {
    let feet = Math.floor((height / 10) * 3.28084);
    let inches = Math.round(((height / 10) * 3.28084 - feet) * 12);
    return `${feet}' ${inches.toString().padStart(2, "0")}"`;
  }

  function convertWeight(weight) {
    let lbs = (weight * 0.220462).toFixed(1);
    return `${lbs} lbs`;
  }

  function getGender(genderRate) {
    if (genderRate === -1) {
      return "N/A";
    } else if (genderRate === 0) {
      return "♂";
    } else if (genderRate === 8) {
      return "♀";
    } else {
      return "♂ ♀";
    }
  }

  function getCategory(genera) {
    let englishGenera = genera.find((g) => g.language.name === "en");
    return englishGenera.genus.split(" ")[0];
  }

  function getEvolution(evolutionChain) {
    let evolutionStages = [];
    let current = evolutionChain.chain;

    while (current) {
      evolutionStages.push(current.species);
      if (current.evolves_to.length > 0) {
        current = current.evolves_to[0];
      } else {
        current = null;
      }
    }
    return evolutionStages;
  }

  function playCry(pokemon) {
    let cry = new Audio(pokemon.crySound);
    isShake = true;
    cry.play();
  }
</script>

<div id="pokemon-detail">
  <div class="pokemon-title">
    {#if selectedPokemon}
      <p>
        {selectedPokemon.name}
        #{selectedPokemon.dexNumber.toString().padStart(3, "0")}
      </p>
    {:else}
      <p>Please select a Pokémon!</p>
    {/if}
  </div>

  <!-- not yet selected -->
  {#if !selectedPokemon}
    <div class="pokemon-section">
      <div class="pokemon-image">
        <img
          src="https://i0.wp.com/www.alphr.com/wp-content/uploads/2016/07/whos_that_pokemon.png?fit=1920%2C1080&ssl=1"
          alt="No Pokémon selected"
          class="placeholder"
        />

        <input type="checkbox" class="shiny-check" title="Shiny!" disabled />
      </div>
      <div id="card-section">
        <div id="card-front">
          <div class="placeholder">
            <div>
              <p>Height</p>
              <span>??</span>
            </div>

            <div>
              <p>Weight</p>
              <span>??</span>
            </div>

            <div>
              <p>Gender</p>
              <span>??</span>
            </div>

            <div>
              <p>Category</p>
              <span>??</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <TypeDetail types={null} />
    <PokemonEntry pokemonDetails={null} />

    <!-- selected but fetching -->
  {:else if !pokemonDetails || !cardDetails || !speciesDetails || !evolutionChain}
    <div id="loading-placeholder">
      <img src="/images/pokeball-placeholder.svg" alt="pokeball-placeholder" />
      <p id="loading">
        LOADING
        <span>.</span>
        <span>..</span>
        <span>...</span>
      </p>
    </div>

    <!-- everything fetched -->
  {:else}
    <div class="pokemon-section">
      <div class="pokemon-image">
        <img
          src={pokemonDetails.normalImage}
          alt={pokemonDetails.name}
          id="normal"
          onload={() => (imageLoaded = true)}
          class:shake={isShake}
          onclick={() => playCry(pokemonDetails)}
          onanimationend={() => (isShake = false)}
        />

        <input type="checkbox" class="shiny-check" title="Shiny!" disabled={!imageLoaded} />

        <img
          src={pokemonDetails.shinyImage}
          alt={pokemonDetails.name}
          id="shiny"
          class:shake={isShake}
          onclick={() => playCry(pokemonDetails)}
          onanimationend={() => (isShake = false)}
        />
      </div>

      <!-- INFO / EVOLUTION CARD -->
      <div id="card-section">
        <div id="card-front">
          <div id="pokemon-info">
            <div>
              <p>Height</p>
              <span>{convertHeight(cardDetails.height)}</span>
            </div>

            <div>
              <p>Weight</p>
              <span>{convertWeight(cardDetails.weight)}</span>
            </div>

            <div>
              <p>Gender</p>
              <span>{getGender(speciesDetails.gender_rate)}</span>
            </div>

            <div>
              <p>Category</p>
              <span>{getCategory(speciesDetails.genera)}</span>
            </div>

            <div>
              <label for="evolution-check"> VIEW EVOLUTION → </label>
            </div>
          </div>
        </div>

        <div id="card-back">
          {#each getEvolution(evolutionChain) as stage}
            <div class="evolution-stage">
              <img
                src={`${PUBLIC_POKEMON_EVOLUTION_URL}/${stage.url
                  .split("/")
                  .at(-2)
                  .padStart(3, "0")}.png`}
                alt={stage.name}
              />
              <span>{stage.name}</span>
            </div>

            {#if stage != getEvolution(evolutionChain).at(-1)}
              <div>
                <p>↓</p>
              </div>
            {/if}
          {/each}
          <label for="evolution-check"> ← BACK </label>
        </div>
        <input type="checkbox" id="evolution-check" />
      </div>
    </div>
    <TypeDetail types={pokemonDetails.types}/>
    <PokemonEntry {pokemonDetails} />
  {/if}
</div>

<style>
  #pokemon-detail {
    display: flex;
    flex-direction: column;
    padding: 0 20px;
    & .pokemon-title p {
      font-size: 50px;
      font-weight: bold;
      color: #56494c;
      font-family: pokemon;
      margin: 10px 0;
      filter: drop-shadow(5px 3px 0 rgba(132, 126, 137, 0.35));
    }
    & .pokemon-section {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 20px;
      margin-bottom: 20px;
      background-color: rgba(255, 255, 255, 0.6);
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.4);
      padding: 20px;
    }

    & #card-front,
    #card-back {
      background-color: #c6c2bc;
      border-radius: 10px;
      width: auto;
      height: 300px;
      padding: 10px 20px;
      box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.4);
    }

    & #card-front {
      display: flex;
      flex-direction: column;
      position: relative;
      & p {
        font-size: 34px;
        font-family: pokemon;
        margin: 0;
        filter: drop-shadow(2px 2px 0 rgba(255, 255, 255, 0.4));
      }
      & span {
        font-size: 20px;
        color: white;
      }

      & #pokemon-info {
        display: flex;
        flex-direction: column;
        gap: 5px;
      }
    }

    & #card-back {
      display: flex;
      flex-direction: column;
      & .evolution-stage {
        display: flex;
        justify-content: center;
        align-items: center;
        flex: 1;
        min-height: 0;
      }
      & img {
        height: 100px;
        max-height: 100%;
        object-fit: contain;
        filter: drop-shadow(3px 3px 0 rgba(0, 0, 0, 0.4));
      }
      & span {
        font-family: pokemon;
        font-size: 30px;
        filter: drop-shadow(2px 2px 0 rgba(255, 255, 255, 0.4));
      }
      & p {
        margin: 0;
        font-size: 30px;
        font-weight: 600;
        line-height: 1.15;
        color: white;
        text-align: center;
        filter: drop-shadow(2px 2px 0 rgba(0, 0, 0, 0.4));
      }
    }

    & #card-section label {
      font-size: 12px;
      color: rgb(70, 65, 65);
      text-decoration: underline;
      display: block;
      text-align: right;
      position: absolute;
      bottom: 8px;
      right: 10px;
      &:hover {
        cursor: pointer;
        color: white;
      }
    }
  }

  #loading {
    font-family: pokemon;
    text-align: center;
    font-size: 50px;
    position: relative;
  }

  #loading span {
    opacity: 0;
    position: absolute;
    left: 105%;
  }

  #loading span:nth-child(1) {
    animation: dot1 0.9s infinite;
  }

  #loading span:nth-child(2) {
    animation: dot2 0.9s infinite;
  }

  #loading span:nth-child(3) {
    animation: dot3 0.9s infinite;
  }

  @keyframes dot1 {
    0%,
    25% {
      opacity: 1;
    }
    26%,
    100% {
      opacity: 0;
    }
  }

  @keyframes dot2 {
    0%,
    25% {
      opacity: 0;
    }
    26%,
    50% {
      opacity: 1;
    }
    51%,
    100% {
      opacity: 0;
    }
  }

  @keyframes dot3 {
    0%,
    50% {
      opacity: 0;
    }
    51%,
    75% {
      opacity: 1;
    }
    76%,
    100% {
      opacity: 0;
    }
  }

  #evolution-check {
    display: none;
  }

  #card-section {
    display: grid;
    justify-content: center;
    align-items: center;

    & #card-front,
    #card-back {
      grid-area: 1 / 1;
      backface-visibility: hidden; /* If it flipped to the back, no content is shown. */
      transition: transform 0.6s ease;
    }

    & #card-back {
      transform: rotateY(180deg);
    }

    &:has(#evolution-check:checked) #card-front {
      transform: rotateY(180deg);
    }
    &:has(#evolution-check:checked) #card-back {
      transform: rotateY(0deg);
    }
  }

  .placeholder {
    opacity: 0.3;
  }

  .pokemon-image {
    /*parent*/
    position: relative;
    /* reference of absolute position */
    display: grid;
    width: auto;

    & img {
      position: relative;
      grid-area: 1 / 1;
      /* put both images in the first row and column. */
      justify-self: center;
      width: 350px;
      object-fit: contain;
      &:not(.placeholder){
        top: -10px;
      }
    }

    & #normal {
      opacity: 1;
      transition: opacity 0.5s ease;
      filter: drop-shadow(0 8px 12px rgba(0, 0, 0, 0.2));
    }

    & #shiny {
      opacity: 0;
      transition: opacity 0.5s ease;
      filter: drop-shadow(0 8px 10px gold);
    }

    & .shiny-check {
      position: absolute;
      top: 5px;
      right: 30px;
      appearance: none;
      background-image: url(/images/star-64.png);
      width: 40px;
      height: 40px;
      background-size: contain;
      /* fit the whole image inside the checkbox. */
      z-index: 1;
      /* keep the checkbox on top */
    }

    & .shiny-check:active {
      animation: shiny-check 0.3s ease;
    }

    &:has(.shiny-check:checked) {
      & .shiny-check {
        /* add a glow effect to highlight the checked state. */
        filter: drop-shadow(10px 5px 2px gold);
      }

      & #normal {
        opacity: 0;
        /* when the checkbox is checked, the normal image disappears and the shiny one appears. */
      }

      & #shiny {
        opacity: 1;
      }
    }
  }

  #loading-placeholder {
    display: flex;
    gap: 10px;
    & img {
      width: 30px;
      opacity: 0.4;
    }
  }

  .shiny-check:hover {
    cursor: pointer;
  }

  .shiny-check:disabled {
    opacity: 0.3;
    cursor: not-allowed;
  }

  @keyframes shiny-check {
    /* Name of animation */
    0% {
      transform: scale(1);
      /* Scale the checkbox down, then back up. */
    }

    30% {
      transform: scale(1.5);
    }

    60% {
      transform: scale(1.3);
    }

    100% {
      transform: scale(1);
    }
  }

  #normal:hover,
  #shiny:hover {
    cursor: url("/images/pokeball.png"), pointer;
  }

  @keyframes pokemonShake {
    0% {
      transform: rotate(0deg);
    }
    25% {
      transform: rotate(-5deg);
    }
    50% {
      transform: rotate(5deg);
    }
    75% {
      transform: rotate(-5deg);
    }
    100% {
      transform: rotate(0deg);
    }
  }

  .shake {
    animation: pokemonShake linear 0.5s;
  }

  @media (max-width: 768px) {
    .pokemon-section {
      flex-direction: column;
    }

    .pokemon-image {
      width: 100%;
      max-width: 400px;
    }

    .pokemon-image img {
      width: 87.5%;
      max-width: 350px;
    }
  }
</style>
