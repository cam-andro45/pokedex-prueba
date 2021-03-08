<script>
    import { createEventDispatcher } from "svelte";
  
    const dispatch = createEventDispatcher();
  
    const closeModal = () => dispatch("closemodal");
  </script>
  
  <div class="back" on:click={closeModal} />
  <div class="modal">
    <header>
      <slot name="header">TEST</slot>
    </header>
    <div class="content">
      <slot />
    </div>
    <footer>
      <slot name="footer">
        <button on:click={closeModal}>Close</button>
      </slot>
    </footer>
  </div>