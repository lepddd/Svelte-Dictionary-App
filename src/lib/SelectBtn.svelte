<script>
  import { createEventDispatcher } from "svelte";

  export let speechs = [];
  export let activeItem = 0;

  const dispatch = createEventDispatcher();
</script>

<div>
  {#each speechs as speech, index}
    <button
      style="width: calc(100% / {speechs.length});"
      on:click={() => dispatch("select", index)}
      class:active={activeItem === index}>{speech}</button
    >
  {/each}
</div>

<style>
  div {
    height: 30px;
    position: absolute;
    background-color: #fff;
    width: 100%;
    left: 0;
    bottom: -30px;
    border-radius: 0 0 5px 5px;
  }
  button::before {
    display: none;
    content: "";
    position: absolute;
    width: 100%;
    height: 3px;
    background-color: #6242ff;
    top: 0;
    left: 0;
  }
  button.active::before {
    display: block;
  }
  button.active {
    color: #6242ff;
  }
  button:first-child {
    border-radius: 0 0 0 5px;
  }
  button:last-child {
    border-radius: 0 0 5px 0;
  }
  button {
    position: relative;
    height: 100%;
    background-color: #fff;
    border: none;
    cursor: pointer;
    font-weight: 500;
    font-size: 14px;
    color: #343a40;
  }
</style>
