<script>
import { prevent_default } from "svelte/internal";
import axios from 'axios';

    let pokemonName
    export let pokemonId
    export let pokemon
    export let loading
    export let error;

    const handleSubmit = async e => {
        if(pokemonName !== ''){
            error = false
            loading = true
            pokemonId = window.isNaN(parseInt(pokemonName)) ? pokemonName.toLowerCase() : pokemonName
            pokemonName = '';
            
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
            return
        }
        error = true
    }
</script>

<form class="pokemon-form" on:submit|preventDefault={handleSubmit}>
    <input
        class="pokemon-input"
        type="text"
        name="pokemon"
        bind:value={pokemonName}
        placeholder="Busca tu pokemon"
        autocomplete="off"/>
    <input type="submit" class="pokemon-btn" value=""/>
</form>

<style>
    .pokemon-form {
      display: grid;
      grid-template-columns: 1fr 50px;
      column-gap: .5rem;
      margin-top: 1em;
    }
    .pokemon-input{
      font-family: system-ui;
      padding: 10px;
      background-color: var(--orange);
      border: none;
      border: 1px solid black;
      border-radius: 10px;
      outline: 0;
      font-size: 1em;
      flex-grow: 1;
      transition: .2s;
    }
    .pokemon-input:focus {
      border-color: #ffffff;
    }
    .pokemon-input:focus::-webkit-input-placeholder{
      color: rgba(255,255,255, 1);
    }
    .pokemon-input::-webkit-input-placeholder {
      color: rgba(255,255,255, .8);
    }
    .pokemon-btn{
      border-radius: 50%;
    }
    .pokemon-btn {
      width: 40px;
      height: 40px;
      position: relative;
      background: radial-gradient(hsl(0, 100%, 70%), hsl(0, 97%, 25%)); /* Standard syntax */
      text-shadow: 0 -1px 0 #c30707;
      color: white;
      border: solid 1px hsl(0, 100%, 20%);
      border-radius: 100%;
      z-index: 1;
      outline: none;
      box-shadow: inset 0 .5px   0 hsl(0, 100%, 50%),
                        0 1px 0 hsl(0, 100%, 20%),
                        0 1.5px   0 hsl(0, 100%, 18%),
                        0 2px 0 hsl(0, 100%, 16%),
                        0 2.5px   0 hsl(0, 100%, 14%),
                        0 3px 0 hsl(0, 100%, 12%),
                        0 3.5px   0 hsl(0, 100%, 10%),
                        0 4px 0 hsl(0, 100%, 8%),
                        0 4.5px   0 hsl(0, 100%, 6%);
    }
    .pokemon-btn:hover {
      background: radial-gradient(hsl(0, 100%, 70%), hsl(0, 97%, 35%)); /* Standard syntax */
    }
    .pokemon-btn:active {
      background: radial-gradient(hsl(0, 100%, 80%), hsl(0, 100%, 45%)); /* Standard syntax */
      top: 2px;
      box-shadow: inset 0 .5px 0 hsl(0, 100%, 50%),
                        0 1px 0 hsl(0, 100%, 20%),
                        0 1.5px 0 hsl(0, 100%, 18%),
                        0 2px 0 hsl(0, 100%, 16%),
                        0 2.5px 0 hsl(0, 100%, 14%),
                        0 3px 0 hsl(0, 100%, 12%),
                        0 3.5px 0 hsl(0, 100%, 10%);
    }
  </style>