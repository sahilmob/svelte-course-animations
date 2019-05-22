<script>
  import { tweened } from "svelte/motion";
  import { cubicIn } from "svelte/easing";
  import Spring from "./Spring.svelte";
  import { fade, fly, slide, scale } from "svelte/transition";
  import { flip } from "svelte/animate";

  let showParagraph = false;
  const progress = tweened(0, {
    delay: 0,
    duration: 700,
    easing: cubicIn
  });
  setTimeout(() => {
    progress.set(0.5);
  }, 1500);

  let boxes = [];

  function addBox() {
    boxes = [Math.random(), ...boxes];
  }

  function discard(value) {
    boxes = boxes.filter(el => el !== value);
  }
</script>

<style>
  div {
    width: 10rem;
    height: 10rem;
    background: #ccc;
    margin: 1rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    border-radius: 5px;
    padding: 1rem;
  }
</style>

<!-- <progress value={$progress} /> -->

<!-- <Spring /> -->

<button on:click={() => (showParagraph = !showParagraph)}>Toggle</button>
<button on:click={addBox}>Add box</button>
{#if showParagraph}
  <p in:fade out:fly={{ x: 300 }}>Can you see me?</p>
{/if}

{#if showParagraph}
  {#each boxes as box (box)}
    <div
      transition:fly={{ easing: cubicIn, x: -300 }}
      on:click={discard.bind(this, box)}
      animate:flip={{ duration: 200, easing: cubicIn }}>
       {box}
    </div>
  {/each}
{/if}
