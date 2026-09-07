<script>

</script>

<div id="header">
    <h1>POKÉDEX</h1>
    <div id="menu">
        <div class="menu-item">
            <div class="menu-title">
                <img src="/images/menu-icons/note.png">
                <span>MUSIC</span>
            </div>
            <div class="drop-down">
            <audio controls>
                <source src="/audio/pokemon-red-blue-lofi.mp3" type="audio/mpeg">
            </audio>
            </div>
        </div>
        <div class="menu-item">
            <div class="menu-title">
                <img src="/images/menu-icons/palette.png">
                <span>THEME</span>
            </div>
            <div class="drop-down">
            <label>
                <input type="radio" id="fire" name="theme">
                <span>FIRE</span>
            </label>

            <label>
                <input type="radio" id="water" name="theme">
                <span>WATER</span>
            </label>

            <label>
                <input type="radio" id="grass" name="theme">
                <span>GRASS</span>
            </label>
            <label>
                <input type="radio" id="default" name="theme">
                <span>DEFAULT</span>
            </label>
            </div>
        </div>
        <div class="menu-item">
            <div class="menu-title">
                <img src="/images/menu-icons/help.png">
                <span>GUIDE</span>
            </div>
            <div class="drop-down">
                <p>♫ Play music from the Music menu</p>
                <p>✎ Choose a theme from the Theme menu</p>
                <p>★ Click the star to view Shiny Pokémon</p>
                <p>⇪ Hover over the type to view strengths & weaknesses</p>
            </div>
        </div>
    </div>
</div>

<style>
    #header{
        background-color: #56494C;
        margin-bottom: 10px;
        display: flex;
        justify-content: space-between;
        align-items: stretch;
        & h1{
            margin: 10px;
            color: #FFCB05;
            font-family: pokemon;
            font-size: 50px;
            padding: 5px 10px;
            text-shadow: #3C5AA6;
            filter: drop-shadow(3px 3px 0 #3C5AA6)
                    drop-shadow(-3px -3px 0 #3C5AA6)
                    drop-shadow(3px 5px 2px rgba(0,0,0,0.3));
            cursor: default;

        }
        & #menu {
            display: flex;
            width: 510px;
            & .menu-item{
                flex: 1;
                color: white;
                padding: 12px 0;
                font-size: 20px;
                position: relative;
                display: flex;
                justify-content: center;
                align-items: center;
                
                & .menu-title{
                    display: flex;
                    gap: 5px;
                    cursor: pointer;
                }


                & img{
                    width: 20px;
                    object-fit: contain;
                }

                &:hover{
                    background-color: rgba(0,0,0, 0.2);
                    & .drop-down{
                        opacity: 1;
                        visibility: visible;
                    }
                    & img{
                        animation: rotate 2s linear infinite;

                    }
                }

                & .drop-down{
                    z-index: 1;
                    opacity: 0;
                    position: absolute;
                    top: 100%;
                    left: 50%;
                    transform: translateX(-50%);
                    visibility: hidden;
                    background-color: rgba(69, 59, 61, 0.9);
                    width: 100%;
                    & audio{
                        width: 100%;
                        height: 40px;
                        padding: 10px;
                    }

                }   
            }
            & .menu-item:nth-child(2){
                & .drop-down{
                    display: flex;
                    flex-direction: column;
                    & label{
                        font-size: 18px;
                        cursor: pointer;
                        height: 35px;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        &:hover{
                            background-color: rgba(0,0,0, 0.2);
                        }
                        &:hover:nth-child(1){
                            color: #EE8130;
                        }
                        &:hover:nth-child(2){
                            color: #6390F0;
                        }
                        &:hover:nth-child(3){
                            color: #7AC74C;
                        }
                        &:hover:nth-child(4){
                            color: #847E89;
                        }
                    }
                    & input{
                        display: none;
                    }
                }
            }
            & .menu-item:nth-child(3){
                & .drop-down{
                    display: flex;
                    flex-direction: column;
                    align-items: flex-start;
                    z-index: 2; /*put it above the shiny button */
                    & p{
                        font-size: 16px;
                        padding: 10px;
                        margin: 0;
                        line-height: 1.15;
                        &:hover{
                            background-color: rgba(0,0,0, 0.2);
                        }
                    }
                }
            }
        }    
    }

</style>