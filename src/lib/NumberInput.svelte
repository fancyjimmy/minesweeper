<script>
  export let value = 0;
  export let min = 0;
  export let max = 100;
  export let step = 1;
  function increase() {
    if (value + step > max) {
      return;
    }
    value += step;
  }

  function decrease() {
    if (value - step < min) {
      return;
    }
    value -= step;
  }
</script>

<div class="number-input">
  <button on:click={decrease} class="l number-input-arrow">🢀</button>
  <div class="number">{value}</div>
  <button on:click={increase} class="r number-input-arrow">🢂</button>
</div>

<style>
  .number-input {
    font-family: "Press Start 2P", cursive;
    display: grid;
    grid-template-columns: 2em 1fr 2em;
    place-items: center;
  }

  .number-input-arrow {
    background: none;
    border: none;
  }
</style>
