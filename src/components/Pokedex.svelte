<script>
    import PokedexScreen from './PokedexScreen.svelte'
    import PokemonForm from './PokemonForm.svelte'
    import axios from 'axios'
    import App2 from '../App2.svelte'

    import { onMount } from 'svelte'

    let error = false;
    let loading = false;
    let pokemon = null;
    const RandomId = Math.floor(Math.random()*806 + 1);
    let pokemonId = RandomId.toString();

    const getPokemon = async () => {
        try {
            const req = await axios('https://pokeapi.co/api/v2/pokemon/'+pokemonId);
            pokemon = req.data;
            console.log(pokemon)
            loading = false;
        } catch (err) {
            console.log(error)
            loading = false;
            error = true;
            pokemon=null;
        }
    }

    onMount(async ()=>{
        try {
            await fetch('https://pokeapi.co/api/v2/pokemon/'+pokemonId)
                .then(res => res.json())
                .then(data => {
                    pokemon=data;
                    loading=false;
                })
                .catch(err=>{
                    loading=false;
                    error=true
                })
            //pokemon = req.data;
            //loading = false;
        } catch (err) {
            console.log(err)
            loading = false;
            error = true;
            pokemon=null;
        }
    })

    /*$: if(pokemonId){
        getPokemon();
    }*/
</script>

<div class="pokedex">
    <div class="pokedex-left">
        <div class="pokedex-left-top">
            <div class={`light is-sky is-big ${loading?'is-animated':''}`}/>
            <div class="light is-red"/>
            <div class="light is-yellow"/>
            <div class="light is-green"/>
        </div>
        <div class="pokedex-screen-container">
            <PokedexScreen
                pokemon={pokemon}
                loading={loading}
                error={error}
            />
        </div>
        <div class="pokedex-left-bottom">
            <div class="pokedex-left-bottom-lights">
                <div class="light is-blue is-medium"/>
                <div class="light is-green is-large"/>
                <div class="light is-orange is-large"/>
            </div>
            <PokemonForm
                bind:pokemonId
                bind:loading
                bind:error
                bind:pokemon
            />
        </div>
    </div>
    <div class="pokedex-right-front"/>
    <div class="pokedex-right-back"/>
</div>

<style>
    .pokedex {
        perspective: 1000px;
        position: relative;
        overflow: hidden;
        height: 31rem;
        width: 23rem;
        margin: 0 auto;
        overflow: initial;
        box-shadow: 0 2px 12px -2px rgba(255,0,0,0.4);
    }
  
    .pokedex:hover .pokedex-right-front {
    transform: rotateY(180deg);
    }

    .pokedex:hover .pokedex-right-back {
    transform: rotateY(0);
    }

    .pokedex-right-front {
    background: var(--red);
    height: calc(100% - 6rem);
    width: inherit;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    transition: 1s;
    transform-origin: right center;
    z-index: 2;
    backface-visibility: hidden;
    border: 1px solid black;
    box-sizing: border-box;
    border-radius: 10px 0 0px 10px;
    border-left: 10px solid #9b0a21;
    }

    .pokedex-right-front:before {
    height: 5rem;
    position: absolute;
    bottom: 100%;
    background: var(--red);
    width: calc(100% - 9rem);
    right: 0;
    border-top: 1px solid black;
    content: '';
    }

    .pokedex-right-front:after {
    content: '';
    border-right: 2.5rem solid var(--red);
    border-bottom: 2.5rem solid var(--red);
    border-left: 2.5rem solid transparent;
    border-top: 2.5rem solid transparent;
    position: absolute;
    right: calc(100% - 9rem);
    bottom: 100%;
    filter: drop-shadow(0 -1px 0 black);
    }

    .pokedex-right-back {
    background: var(--red);
    height: calc(100% - 6rem);
    width: inherit;
    position: absolute;
    backface-visibility: hidden;
    transform: rotateY(-180deg);
    right: 0;
    bottom: 0;
    transition: 1s;
    left: 100%;
    transform-origin: left top;
    z-index: 1;
    border-width: 1px;
    border-style: solid;
    border-color: black;
    border-radius: 0 10px 10px 0;
    border-left: none;
    padding: 20px;
    box-sizing: border-box;
    display: flex;
    }

    .pokedex-right-back:before {
    width: calc(100% - 9rem);
    height: 5rem;
    position: absolute;
    bottom: 100%;
    background-color: var(--red);
    left: 0;
    border-top: 1px solid black;
    content: '';
    }

    .pokedex-right-back:after {
    content: '';
    border-left: 2.5rem solid var(--red);
    border-bottom: 2.5rem solid var(--red);
    border-top: 2.5rem solid transparent;
    border-right: 2.5rem solid transparent;
    display: block;
    width: 0;
    position: absolute;
    left: calc(100% - 9rem);
    bottom: 100%;
    filter: drop-shadow(0 -1px 0 black)
    }

    .light {
    border: 1px solid black;
    box-shadow: -2px 2px 0 rgba(255,255,255, .5);
    width: 20px;
    height: 20px;
    background: gray;
    border-radius: 50%;
    }

    .light.is-animated {
    animation: .3s light linear infinite;
    }

    @keyframes light {
    0% {
        background-color: white;
    }
    50% {
        background-color: var(--sky);
    }
    100% {
        background-color: white;
    }
    }

    .light.is-big {
    width: 50px;
    height: 50px;
    }

    .light.is-medium {
    width: 40px;
    height: 40px;
    }
    .light.is-large {
    width: 80px;
    border-radius: 20px;
    }

    .light.is-red {
    background-color: var(--red);
    }
    .light.is-blue {
    background-color: var(--blue);
    }
    .light.is-green {
    background-color: var(--green);
    }
    .light.is-sky {
    background-color: var(--sky);
    }
    .light.is-orange {
    background-color: var(--orange);
    }

    .light.is-yellow {
    background-color: var(--yellow);
    }

    .pokedex-left {
    background: var(--red);
    height: inherit;
    width: inherit;
    border-radius: 10px 0 0 10px;
    border: 1px solid black;
    border-right: 10px solid black;
    box-sizing: border-box;
    padding: 15px 20px;
    }

    .pokedex-left-top {
    display: flex;
    align-items: flex-start;
    }

    .pokedex-left-top > * {
    margin-right: .7em;
    }

    .pokedex-left-bottom-lights {
    display: flex;
    align-items: flex-start;

    }

    .pokedex-left-bottom {
    margin-top: 1em;
    }

    .pokedex-left-bottom-lights  > * {
    margin-right: .8em;
    }

</style>