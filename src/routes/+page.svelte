<script lang="ts">
  import App from "$lib/components/App.svelte";
</script>

<div>
  <App />
</div>

<style>
  :global(body) {
    margin: 0;
    touch-action: none;
  }

  div {
    width: 100vw;
    height: 100vh;
    background: hsl(114, 43%, 14%);
    background: linear-gradient(180deg, hsl(126, 43%, 14%) 0%, hsl(120, 45%, 7%) 100%);
  }
</style>
