<script>
  import MField from "./lib/MField.svelte";
  import LoseScreen from "./lib/LoseScreen.svelte";
  import MChoose from "./lib/MChoose.svelte";
  import { fade, fly } from "svelte/transition";
  import WinScreen from "./lib/WinScreen.svelte";

  let lost = false;
  let won = false;
  let choose = true;
  let mode = "";

  let rows = 20;
  let columns = 20;
  let chance = 0.16;

  let score = { time: 0, clicks: 0 };
  //TODO return to select mode when lost
  //TODO fix timer -> don't use getSeconds
  //TODO refactor css
  //TODO make responsive
  //TODO MAYBE leaderboard / score in localstorage

  const start = () => {
    if (lost) {
      document.body.classList.remove("lose");
    }
    if (won) {
      document.body.classList.remove("won");
    }
    lost = false;
    won = false;
    choose = false;
    score = { time: 0, clicks: 0 };
  };
  function startPlay(e){
    rows = e.detail.rows;
    columns = e.detail.columns;
    chance = e.detail.chance;
    mode = e.detail.mode;
    start();
  }
</script>

<main>
  {#if !lost && !won && !choose}
    <div class="center">
      <MField
        bind:rows
        bind:columns
        bind:chance
        on:lose={() => {
          setTimeout(() => {
            document.body.classList.toggle("lose");
            lost = true;
          }, 500);
        }}
        on:win={(e) => {
          setTimeout(() => {
            document.body.classList.toggle("won");
            score = { clicks: e.detail.clicked, time: e.detail.time };
            won = true;
          }, 500);
        }}
      />
    </div>
  {:else if choose}
    <div class="center">
      <MChoose on:choose={startPlay} />
    </div>
  {:else if won}
    <div class="center" in:fade={{ duration: 1000 }}>
      <WinScreen replayCB={start} {score} />
    </div>
  {:else}
    <div class="center">
      <LoseScreen replayCB={start} />
    </div>
  {/if}
</main>

<style>
  @font-face {
    font-family: "Press Start 2P";
    src: url("./assets/PressStart2P-Regular.ttf");
  }
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  .center {
    display: grid;
    place-items: center;
    width: 100vw;
    height: 100vh;
  }

  main {
    text-align: center;
    margin: 0 auto;
    width: 100vw;
    height: 100vh;
  }

  @media (min-width: 480px) {
  }
</style>
