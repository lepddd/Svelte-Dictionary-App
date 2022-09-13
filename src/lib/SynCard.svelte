<script>
  import { createEventDispatcher } from "svelte";

  export let title = "title";
  export let synonyms =
    "pleasure, entertainment, enjoiment, amusement, excitement.";

  const dispatch = createEventDispatcher();

  const newSearch = (param) => {
    if (param !== "No existing synonyms!") {
      dispatch("searchSyn", param);
    }
  };
</script>

<div class="container">
  <div class="container-body">
    <p class="title">{title}</p>
    {#each synonyms as synonym}
      <span
        class="body"
        style="cursor: pointer; text-decoration: underline; margin-right: 5px;text-aling:center;"
        on:click={() => newSearch(synonym)}
        >{synonym}
      </span>
    {/each}
  </div>
</div>

<style>
  .container {
    padding-left: 3px;
    background-color: #6242ff;
    text-align: left;
    border-radius: 5px;
  }
  .container-body {
    background-color: #fff;
    border-radius: 5px;
    padding: 5px 10px;
  }
  .title {
    color: #343a40;
    font-weight: 500;
    font-size: 16px;
  }
  .body {
    color: #6c757d;
  }
</style>
