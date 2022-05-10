<script>
  import flag from "../assets/flag.png";
  export let score = { time: 0, clicks: 0 };
  export let replayCB = () => {};
</script>

<div class="win-screen">
  <h2>YOU WON!!!</h2>
  <h3>Your Score: {Math.round(score.time)}</h3>
  <div class="win-cont" on:click={replayCB}>
    <img src={flag} alt="EXPLOSION" class="flag" />
  </div>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

  h2 {
    color: rgb(168, 255, 168);
    text-shadow: -0.2em 0.2em black;
  }

  .win-screen {
    background-color: #c0c0c0;
    font-family: "Press Start 2P", cursive;

    display: grid;
    grid-template-rows: 1fr 1fr 9fr;
    place-items: center;
    width: min(80vw, 80vh);
    height: min(80vw, 80vh);

    border: 0.5em outset #adadad;
    padding: 1.5em;
    border-top: outset 0.5em #ffffff;
    border-left: outset 0.5em #ffffff;
  }

  .win-cont {
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

  @keyframes r {
    0% {
      transform: rotateY(0deg);
    }
    100% {
      transform: rotateY(720deg);
    }
  }

  .flag {
    image-rendering: pixelated;
    height: 80%;
    aspect-ratio: 1;
    filter: drop-shadow(-0.2em 0.2em 0 black);
    animation: ease-in r 1s;
  }
</style>
