<script>
  import logo from "./assets/svelte.png";
  import Counter from "./lib/Counter.svelte";
  import MField from "./lib/MField.svelte";
  import LoseScreen from "./lib/LoseScreen.svelte";
  import { fade, fly } from "svelte/transition";
  import WinScreen from "./lib/WinScreen.svelte";

  let lost = false;
  let won = false;

  let score = { time: 0, clicks: 0 };

  const start = () => {
    if (lost) {
      document.body.classList.remove("lose");
    }

    if (won) {
      document.body.classList.remove("won");
    }
    lost = false;
    won = false;
    score = { time: 0, clicks: 0 };
  };
</script>

<main>
  {#if !lost && !won}
    <div class="center">
      <MField
        rows={20}
        columns={20}
        chance={0.1}
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
