<script>
  import { createEventDispatcher } from "svelte";
  import flag from "../assets/flag.png";
  import mine from "../assets/mine.png";

  export let isBomb = false;
  export let surroundingCount = 6;
  export let row;
  export let col;

  let exposed = false;

  const dispatch = createEventDispatcher();

  let flagged = false;

  function toggle(e) {
    e.preventDefault();
    if (exposed) {
      return;
    }
    flagged = !flagged;
  }

  export function expose() {
    exposed = true;
    flagged = false;
    dispatch("expose", { col, row, isBomb, surroundingCount });
  }

  export function bomb() {
    return isBomb;
  }

  export function isExposed() {
    return exposed;
  }

  const colorMap = {
    "0": "#000000",
    "1": "#0100fe",
    "2": "#017f00",
    "3": "#fe0000",
    "4": "#010080",
    "5": "#810002",
    "6": "#008081",
    "7": "#000000",
    "8": "#808080",
  };

  function getColor(surroundingCount) {
    return colorMap[surroundingCount.toString()];
  }
</script>

<div
  class:exposed
  class="m-cell"
  style:color={getColor(surroundingCount)}
  style="--col: {(row + col) % 2 === 0 ? '#00DD7F' : '#ADFF2F'}"
  on:click|once={() => {
    if (!exposed) {
      expose();
      dispatch("pressed");
    }
  }}
  on:contextmenu={toggle}
>
  {#if exposed}
    {#if isBomb}
      <img src={mine} alt="B" class="mine" />
    {:else}
      {surroundingCount !== 0 ? surroundingCount : ""}
    {/if}
  {/if}

  {#if flagged}
    <img src={flag} alt="F" class="flag" />
  {/if}
</div>

<style>
  .flag,
  .mine {
    width: 90%;
    height: 90%;
  }
  .m-cell {
    overflow: hidden;
    background-color: var(--col);
    background-color: #c0c0c0;
    display: grid;
    place-items: center;
    aspect-ratio: 1;
    border: 3px outset #adadad;
    border-top: outset 3px #ffffff;
    border-left: outset 3px #ffffff;
  }
  .m-cell:hover {
    filter: brightness(1.2);
  }

  .exposed {
    background-color: #c0c0c0;
    border: 1px solid rgb(109, 109, 109);
  }
</style>
