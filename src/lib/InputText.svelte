<script>
  import Icon from "@iconify/svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let value = "";

  $: checkInput = value === "" ? false : true;

  const acceptedKeys = {
    Enter(val) {
      dispatch("press", val);
    },
    Backspace(val) {
      if (val <= 0) {
        dispatch("press", val);
      }
    },
    Delete(val) {
      if (val <= 0) {
        dispatch("press", val);
      }
    },
  };

  const handlePress = (e) => {
    const keyPressed = acceptedKeys[e.key];
    if (keyPressed) {
      keyPressed(value);
    }
  };

  const clearInput = () => {
    value = "";
    dispatch("clear");
  };
</script>

<div>
  <input
    type="text"
    bind:value
    on:keyup={(e) => handlePress(e)}
    placeholder="Search a word"
  />
  <Icon class="icon search" icon="akar-icons:search" width="24" height="24" />
  <span class:active={checkInput} on:click={() => clearInput()}>
    <Icon class="icon close" icon="ci:close-small" width="24" height="24" />
  </span>
</div>

<style>
  div {
    position: relative;
    margin-top: 30px;
    margin-bottom: 10px;
  }
  div input {
    width: 347px;
    height: 50px;
    background-color: #fff;
    border: 2px solid #6c757d;
    border-radius: 5px;
    outline: none;
    padding: 0 54px;
    font-weight: 500;
    font-size: 16px;
  }
  div input::placeholder {
    color: #adb5bd;
  }
  div input:focus {
    border: 2px solid #6242ff;
  }
  div input:focus ~ :global(.search) {
    color: #6242ff;
  }
  div span {
    display: none;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: calc(50% - 18px);
    right: 10px;
    cursor: pointer;
    padding: 6px;
  }
  div :global(.icon) {
    top: calc(50% - 12px);
    color: #6c757d;
  }
  div :global(.search) {
    position: absolute;
    left: 15px;
    pointer-events: none;
  }
  .active {
    display: flex;
  }
</style>
