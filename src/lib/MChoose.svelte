<script>
  import NumberInput from "./NumberInput.svelte";

  export let rows = 20;
  export let columns = 20;
  export let chance = 0.16;

  let customRows = 20;
  let customColumns = 20;
  let customChance = 16;

  let currentMode = 0;

  const mode = [
    { mode: "easy", rows: 8, columns: 8, chance: 0.16 },
    { mode: "normal", rows: 16, columns: 16, chance: 0.16 },
    { mode: "hard", rows: 16, columns: 30, chance: 0.2 },
  ];

  function selectMode(i) {
    currentMode = i;
    rows = mode[i].rows;
    columns = mode[i].columns;
    chance = mode[i].chance;
  }
</script>

<div class="m-choose">
  <div class="fixed-mode">
    <div class="mode header">
      <div class="mode-name">Difficulty</div>
      <div>H</div>
      <div>W</div>
      <div>%</div>
    </div>
    {#each mode as i}
      <div class="mode {i.mode}">
        <div class="mode-name">{i.mode}</div>
        <div class="mode-rows">{i.rows}</div>
        <div class="mode-columns">{i.columns}</div>
        <div class="mode-chance">{Math.round(i.chance * 100)}%</div>
      </div>
    {/each}
  </div>
  <div class="custom-mode-cont">
    custom
    <div class="custom-mode">
      <p>height:</p>
      <NumberInput bind:value={customRows} min={5} max={100} />
      <p>width:</p>

      <NumberInput bind:value={customColumns} min={5} max={100} />
      <p>mine chance:</p>

      <NumberInput bind:value={customChance} min={0} max={100} />
    </div>
    <button class="choose-btn">Select</button>
  </div>
</div>

<style>
  .choose-btn {
    font-family: "Press Start 2P", cursive;
    border: 0.5em outset #adadad;
    border-top: outset 0.5em #ffffff;
    border-left: outset 0.5em #ffffff;
    padding: 0.35em;
    margin: 0.5em;
  }
  .fixed-mode {
    height: 40%;
    display: grid;
    grid-template-rows: repeat(4, 1fr);
    grid-gap: 0.5em;
  }

  .custom-mode-cont {
    padding: 1.5em;
    border: 0.5em outset #adadad;
    border-top: outset 0.5em #ffffff;
    border-left: outset 0.5em #ffffff;
  }

  .easy > .mode-name {
    color: red;
  }

  .custom-mode {
    padding: 0.5em;
    text-align: left;
    display: grid;
    grid-template-columns: 1fr 10em;
    grid-template-rows: repeat(3, 1fr);
    height: 70%;
  }
  .custom-mode p {
    margin-left: 1.5em;
  }
  .mode-rows,
  .mode-columns,
  .mode-chance {
    color: rgb(83, 83, 83);
  }
  .normal > .mode-name {
    color: blue;
  }
  .hard > .mode-name {
    color: green;
  }
  .mode {
    border: 0.5em outset #adadad;
    border-top: outset 0.5em #ffffff;
    border-left: outset 0.5em #ffffff;

    text-align: center;
    display: grid;
    align-items: center;
    padding: 1.5em;
    grid-template-columns: 1fr repeat(3, 3.5em);
  }

  .header {
    padding: 0.5em;
    border: 0.5em outset #ffffff00;
  }
  .header * {
    text-decoration: solid 0.25em;
  }

  .mode-name {
    text-align: left;
  }
  .m-choose {
    font-family: "Press Start 2P", cursive;
    font-size: 0.8em;
    display: grid;
    width: min(80vw, 80vh);
    height: min(80vw, 80vh);
    background-color: #c0c0c0;
    grid-template-rows: 4fr 5fr;

    border: 0.5em outset #adadad;
    padding: 1.5em;
    border-top: outset 0.5em #ffffff;
    border-left: outset 0.5em #ffffff;
  }
</style>
