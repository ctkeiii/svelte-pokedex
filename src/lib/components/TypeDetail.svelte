<script>
  let { types, typeLoaded = $bindable(false) } = $props();
  let typeDetails = $state([]);
  let hoveredType = $state(null);

  async function fetchTypeDetail(types) {
    typeLoaded = false;
    typeDetails = [];
    for (let type of types) {
      let response = await fetch(`https://pokeapi.co/api/v2/type/${type}`);
      let data = await response.json();
      typeDetails.push(data);
    }
    typeLoaded = true;
  }

  $effect(() => fetchTypeDetail(types));

  let damageRelations = {
    no_damage_to: "No effect on",
    half_damage_to: "Resisted by",
    double_damage_to: "Strong against",
    double_damage_from: "Weak to",
    half_damage_from: "Resists",
    no_damage_from: "Immune to"
  };
</script>

<div class="pokemon-type-container">
  {#if !types}
    <div class="placeholder-type-buttons">
      <button class="pokemon-type placeholder" disabled>
        <img src="/images/type-icons/type-icon-fire.png" alt="placeholder">
        <span>????????</span>
      </button>
    </div>
  {:else}
    <div class="type-buttons">
      {#each types as type}
        <button class="pokemon-type" onmouseenter={() => (hoveredType = type.toLowerCase())}>
          <img src={`/images/type-icons/type-icon-${type.toLowerCase()}.png`} alt={type} />
          <span>{type.toUpperCase()}</span>
        </button>
      {/each}
    </div>
    {#each typeDetails as typeDetail}
      {#if hoveredType === typeDetail.name}
        <div class="type-detail">
          <div id="offense">
            <div class="type-title">
              <p>Offense</p>
            </div>
            <div class="type-content">
              {#each Object.entries(typeDetail.damage_relations) as [relation, types]}
                {#if types.length > 0 && relation.includes("_to")}
                  <div class="type-row">
                    <span>{damageRelations[relation]}</span>
                    {#each types as type}
                      <img
                        src={`/images/type-icons/type-icon-${type.name}.png`}
                        title={type.name}
                        alt={type.name}
                      />
                    {/each}
                  </div>
                {/if}
              {/each}
            </div>
          </div>
          <div id="defense">
            <div class="type-title">
              <p>Defense</p>
            </div>
            <div class="type-content">
              {#each Object.entries(typeDetail.damage_relations) as [relation, types]}
                {#if types.length > 0 && relation.includes("_from")}
                  <div class="type-row">
                    <span>{damageRelations[relation]}</span>
                    {#each types as type}
                      <img
                        src={`/images/type-icons/type-icon-${type.name}.png`}
                        title={type.name}
                        alt={type.name}
                      />
                    {/each}
                  </div>
                {/if}
              {/each}
            </div>
          </div>
        </div>
      {/if}
    {/each}
  {/if}
</div>

<style>
  .pokemon-type {
    border: none;
    display: flex;
    align-items: center;
    justify-content: stretch;
    background-color: #6e6b6b;
    border-radius: 15px;
    width: auto;
    height: 30px;
    overflow: hidden; /* Keep all content inside the container */
    font: inherit;

    & img {
      width: 30px;
      object-fit: cover; /* make the image fill the entire area */
      clip-path: polygon(0 0, 95% 0, 70% 100%, 0 100%);
      transform: scale(1.4);
    }

    & span {
      font-size: 16px;
      color: white;
      padding: 10px;
    }

    &:hover {
      cursor: pointer;
    }
  }

  .type-buttons {
    display: flex;
    gap: 10px;
  }

  .pokemon-type-container:hover .type-detail {
    /*If the pokemon-type is hovered, the next sibling type-detail will appear*/
    opacity: 1;
    visibility: visible;
  }

  .pokemon-type-container {
    position: relative;
    width: fit-content;
    max-width: 100%;
    margin-bottom: 10px;
  }

  .type-detail {
    background-color: rgba(81, 77, 104, 0.8);
    position: absolute; /* not occupy the area */
    display: flex;
    color: white;
    border-radius: 15px;
    gap: 25px;
    padding: 20px;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.5s ease;
    bottom: 100%;
    width: auto;

    & #offense,
    #defense {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    & p {
      font-size: 5vh;
      font-weight: bold;
      font-family: pokemon;
      color: #e9b949;
      filter: drop-shadow(0 8px 12px rgba(0, 0, 0, 0.7));
      margin: 0 0 5px 0;
    }
    & img {
      width: 3vh;
      height: 3vh;
    }
    & .type-row {
      display: flex;
      align-items: center; /* Vertically align the text and images. */
      gap: 5px;
      margin-bottom: 5px;
      white-space: nowrap; /* Stay at the same row*/

      & img:hover {
        transform: scale(1.2);
      }
    }
  }
  .placeholder{
    opacity: 0.5;
  }
</style>
