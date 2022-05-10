<script>
  import { onDestroy, onMount } from "svelte";

  export let clicks = 0;
  let startTime = new Date();
  let currentTime = new Date();
  let intervall;
  $: time = currentTime.getSeconds() - startTime.getSeconds();
  onMount(() => {
    startTime = new Date();
    currentTime = new Date();
    intervall = setInterval(() => {
      currentTime = new Date();
    }, 1000);
  });

  export function getTime() {
    return time;
  }

  onDestroy(() => {
    clearInterval(intervall);
  });
</script>

<div class="score-board">
  <div class="score">
    {clicks}
  </div>
  <div />
  <div class="score">
    {time}
  </div>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

  .score-board {
    box-sizing: border-box;
    font-family: "Press Start 2P", cursive;
    width: 100%;
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
    border: 0.4em inset #adadad;
    border-bottom: inset 0.4em #ffffff;
    border-right: inset 0.4em #ffffff;
  }

  .score {
    background: black;
    padding: 0.2em;
    margin: 0.2em;
    border: 0.2em inset #adadad;
    border-bottom: inset 0.2em #ffffff;
    border-right: inset 0.2em #ffffff;

    color: red;
  }
</style>
