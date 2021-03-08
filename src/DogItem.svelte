<script>
    import { createEventDispatcher } from "svelte";
  
    const dispatch = createEventDispatcher();
  
    export let id;
    export let name;
    export let thumbnail;
  </script>
  
  <article on:click={() => dispatch('selectitem', { id })}>
    <h1>{name}</h1>
    <img alt={name} src={thumbnail} />
  </article>