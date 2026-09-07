<script>
    let { pokemon } = $props();

</script>

<div id="pokemon-detail">
    <div id="pokemon-title">
        <p>Pikachu #025</p>
    </div>
      <div id="pikachu-section">
        <div id="pikachu">
            <img src="/images/pokemon/025.png" id="normal">
            <input type="checkbox" id="shiny-check" title="Shiny!">
            <img src="/images/pokemon/025-shiny.png" id="shiny">
            </div>
        <div id="card-section">
          <div id="card-front">
            <div id="pikachu-info">
              <div>
                <p>Height</p>
                <span>1' 04"</span>
              </div>
              <div>
                <p>Weight</p>
                <span>13.2 lbs</span>
              </div>
              <div>
                <p>Gender</p>
                <span>♂ ♀</span>
              </div>
              <div>
                <p>Category</p>
                <span>Mouse</span>
              </div>
              <label for="evolution-check">
                VIEW EVOLUTION →
              </label>
            </div>
          </div>
          <div id="card-back">
            <div class="evolution-stage">
              <img src="https://www.pokemon.com/static-assets/content-assets/cms2/img/pokedex/detail/172.png">
              <span>Pichu</span>
            </div>
            <div>
              <p>↓</p>
            </div>
            <div class="evolution-stage">
              <img src="https://www.pokemon.com/static-assets/content-assets/cms2/img/pokedex/detail/025.png">
              <span>Pikachu</span>
            </div>
            <div>
              <p>↓</p>
            </div>
            <div class="evolution-stage">
              <img src="https://www.pokemon.com/static-assets/content-assets/cms2/img/pokedex/detail/026.png">
              <span>Raichu</span>
            </div>
            <label for="evolution-check">
              ← BACK
            </label>
          </div>
          <input type="checkbox" id="evolution-check">
        </div>
    </div>
</div>


<style>
    #pokemon-detail {
        display: flex;
        flex-direction: column;
        padding: 0 20px;
        & #pokemon-title p {
            font-size: 50px;
            font-weight: bold;
            color: #56494C;
            font-family: pokemon;
            margin: 10px 0;
            filter: drop-shadow(5px 3px 0 rgba(132, 126, 137, 0.35));
        }
        & #pikachu-section{
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            min-height: 400px;
        }

        & #card-front, #card-back{
            background-color:#B8B1B7;
            border-radius: 10px;
            width: 250px;
            height: 300px;
            padding: 10px 20px;

        }

        & #card-front{
            display: flex;
            flex-direction: column;
            position: relative;
            & p{
                font-size: 34px;
                font-family: pokemon;
                margin: 0;
                filter: drop-shadow(2px 2px 0 rgba(255, 255, 255, 0.4));
            }
            & span{
                font-size: 20px;
                color: white;
            }

            & #pikachu-info{
                display: flex;
                flex-direction: column;
                gap: 5px;
            }
        }

        & #card-back{
            display: flex;
            flex-direction: column;
            & .evolution-stage{
                display: flex;
                justify-content: center;
                align-items: center;
                flex: 1;
                min-height: 0;
            }
            & img{
                height: 100px;
                max-height: 100%;
                object-fit: contain;
                filter: drop-shadow(3px 3px 0 rgba(0, 0, 0, 0.4));
            }
            & span{
                font-family: pokemon;
                font-size: 30px;
                filter: drop-shadow(2px 2px 0 rgba(255, 255, 255, 0.4));
            }
            & p{
                margin: 0;
                font-size: 30px;
                font-weight: 600;
                line-height: 1.15;
                color: white;
                text-align: center;
                filter: drop-shadow(2px 2px 0 rgba(0, 0, 0, 0.4))
            }
        }

        & #card-section label{
            font-size: 12px;
            color: rgb(70, 65, 65);
            text-decoration: underline;
            display: block;
            text-align: right;
            position: absolute;
            bottom: 8px;
            right: 20px;
            &:hover{
                cursor: pointer;
                color: white;
            }
        }

    & #pokemon-type{
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: #645b5d;
        border-radius: 15px;
        width: 120px;
        height: 30px;
        overflow: hidden; /* Keep all content inside the container */
        margin: 0 0 5px 0;

        
        & img{
            width: 40px;
            height: 100%;
            background-color: #E9B949;
            object-fit: cover; /* make the image fill the entire area */
            clip-path: polygon(
            0 0,
            100% 0,
            70% 100%,
            0 100%
            );
        }

        & span{
            font-size: 16px;
            color: white;
            padding: 0 18px 0 5px;
        }

        &:hover{
            cursor: pointer;
        }
    }

    & #pokemon-entry{
        background-color: rgb(211, 204, 204);
        border: 1px solid transparent;
        border-radius: 10px;
        padding: 15px;
        margin-bottom: 10px;
        color: rgb(73, 67, 67);
        & p:nth-child(1){
            font-family: pokemon;
            font-size: 30px;
            margin: 0 0 10px 0;
            filter: drop-shadow(2px 2px 0 rgba(0, 0, 0, 0.2));
        }
        & p:nth-child(2){
            margin: 0;
        }
    }
}

    #evolution-check{
        visibility: hidden;
    }

    #card-section{
        display: grid;
        justify-content: center;
        align-items: center;

        & #card-front,#card-back{
            grid-area: 1 / 1;
            backface-visibility: hidden; /* If it flipped to the back, no content is shown. */
            transition: transform 0.6s ease;
        }

        & #card-back{
            transform: rotateY(180deg);
        }

        &:has(#evolution-check:checked) #card-front{
            transform: rotateY(180deg);
        }
        &:has(#evolution-check:checked) #card-back{
            transform: rotateY(0deg);
        }
    }

    #pikachu {
        /*parent*/
        position: relative;
        /* reference of absolute position */
        display: grid;
        width: min(400px, 100%);

        & #normal,
        #shiny {
            grid-area: 1 / 1;
            /* put both images in the first row and column. */
            justify-self: center;
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

        & #shiny-check {
            position: absolute;
            top: 10px;
            right: 10px;
            appearance: none;
            background-image: url(/images/star-64.png);
            width: 40px;
            height: 40px;
            background-size: contain;
            /* fit the whole image inside the checkbox. */
            z-index: 1;
            /* keep the checkbox on top */
        }

        & #shiny-check:active {
            animation: shiny-check 0.3s ease;
        }

        &:has(#shiny-check:checked) {
            & #shiny-check {
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

        & #shiny-check:hover {
            cursor: pointer;

        }
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



</style>