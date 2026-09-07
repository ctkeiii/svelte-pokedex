<script>
    import { onMount } from 'svelte';
    import { PUBLIC_SERVER_BASE_URL } from '$env/static/public';
    let pokemons = $state([]);
    async function fetchPokemonList() {
        let response = await fetch(PUBLIC_SERVER_BASE_URL);
        pokemons = await response.json();
    }
    
    onMount(fetchPokemonList);
</script>

<div id="container">
    <div id="list-section">
        <div id="search-container">
            <input id="search-bar" type="text" placeholder="Search Pokémon">
            <button id="search-button"></button>
        </div>
        <div id="pokemon-list">
            {#each pokemons as pokemon}
                <p><img src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/${pokemon.dexNumber}.png`} alt={pokemon.name}>#{pokemon.dexNumber} {pokemon.name}</p>
            {/each}

        </div>
    </div>
</div>


<style>
    #list-section {
        display: grid;
        grid-template-rows: auto 1fr;
        gap: 10px;
        padding-left: 10px;

        & #search-container{
            position: relative;
            padding: 0 27px 0 3px;

            & #search-bar{
            border: 1px solid transparent;
            border-radius: 10px;
            background-color: #847E89;
            color: white;
            height: 40px;
            padding: 0 50px 0 10px;

            width: 100%;
            &::placeholder{
                color: white;
                }
            }

            & #search-button{
                z-index: 1;
                width: 25px;
                height: 25px;
                position: absolute;
                top: 8px;
                right: 40px;
                background-image: url("/images/search-button.png");
                background-size: contain;
                background-color: transparent;
                border: none;
                &:hover{
                    cursor: pointer;
                }
            }
        }
    }

    #pokemon-list {
        padding-right: 10px;
        /* Create the scrollbar */
        height: 700px;
        overflow-y: auto; /* add a vertical scrollbar when the content overflows. */

        & p {
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
        }

        & .selected {
            background-color: #5c4155c3;
            color: white;
        }

        & img{
            height: 40px;
            width: 40px;
            transition: transform 0.2s ease;
        }
        
        & .small-img{
            transform: scale(1.4);
        }

        & p:hover{
            background-color:#918992c0;
            color: white;
            cursor: pointer;
            img:not(.small-img){
                transform: scale(1.15);
                filter: drop-shadow(0 0 12px grey);
            }
            .small-img{
                transform: scale(1.61);
                filter: drop-shadow(0 0 12px grey);
            }
        }
    }

    #pokemon-list::-webkit-scrollbar {
        width: 15px;
    }

    #pokemon-list::-webkit-scrollbar-track {
        background: #E5E0E1;
        border-radius: 5px;
    }

    #pokemon-list::-webkit-scrollbar-thumb {
        background: rgba(0,0,0, 0.2);
        border-radius: 5px;
    }
</style>