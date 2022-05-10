<script>
  import explosion from "../assets/explosion.png";
  export let replayCB = () => {};
</script>

<div class="lose-screen">
  <h2>YOU LOST...</h2>
  <div class="mine-cont" on:click={replayCB}>
    <img src={explosion} alt="EXPLOSION" class="mine" />
  </div>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

  h2 {
    color: red;
    text-shadow: -0.2em 0.2em black;
  }

  .lose-screen {
    background-color: #c0c0c0;
    font-family: "Press Start 2P", cursive;

    display: grid;
    grid-template-rows: 1fr 9fr;
    place-items: center;
    width: min(80vw, 80vh);
    height: min(80vw, 80vh);

    border: 0.5em outset #adadad;
    padding: 1.5em;
    border-top: outset 0.5em #ffffff;
    border-left: outset 0.5em #ffffff;
  }

  .mine-cont {
    box-sizing: border-box;
    width: 80%;
    height: 80%;

    display: grid;
    place-items: center;

    padding: 0.5em;
    border: 0.5em inset #adadad;
    border-bottom: inset 0.5em #ffffff;
    border-right: inset 0.5em #ffffff;
  }

  .mine {
    image-rendering: pixelated;
    width: 80%;
    aspect-ratio: 1;
    filter: drop-shadow(-0.3em 0.3em 0 0 black);

  }
</style>
