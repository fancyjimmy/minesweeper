<script>
    import MCell from "./MCell.svelte";
    import {createEventDispatcher, onDestroy, onMount} from "svelte";
    import {prevent_default} from "svelte/internal";
    import Score from "./Score.svelte";

    export let rows = 20;
    export let columns = 20;
    export let chance = 0.2;

    let startTime = new Date();
    const dispatch = createEventDispatcher();

    const size = "2em";

    let bombCount = 0;
    let exposed = 0;

    let bombGrid = Array.from({length: rows}, (v, num) => {
        return Array.from({length: columns}, (val2, num2) => {
            let r = Math.random();
            if (r < chance) bombCount++;
            return r < chance;
        });
    });

    function calcSurround(x, y) {
        let count = 0;

        for (let i = x - 1; i <= x + 1; i++) {
            for (let j = y - 1; j <= y + 1; j++) {
                if (i < 0 || i >= bombGrid.length) continue;
                if (j < 0 || j >= bombGrid[0].length) continue;
                if (x === i && y === j) continue;

                if (bombGrid[i][j]) {
                    count++;
                }
            }
        }
        return count;
    }

    let expoGrid = bombGrid.map((v, i) => {
        return v.map((w, j) => {
            return null;
        });
    });

    function exposeSurr(x, y) {
        for (let i = x - 1; i <= x + 1; i++) {
            for (let j = y - 1; j <= y + 1; j++) {
                if (i < 0 || i >= expoGrid.length) continue;
                if (j < 0 || j >= expoGrid[0].length) continue;
                if (!expoGrid[i][j].isExposed()) {
                    expoGrid[i][j].expose();
                }
            }
        }
    }

    function hasWon() {
        let exposed = 0;
        for (let i = 0; i < expoGrid.length; i++) {
            const v = expoGrid[i];
            for (let j = 0; j < v.length; j++) {
                const e = v[j];
                if (!e.isExposed()) {
                    exposed++;
                }
            }
        }
        /*
        expoGrid.forEach((val) => {
          exposed =
            exposed + val.reduce((prev, curr) => prev + (curr.exposed ? 0 : 1), 0);
        });
        */
        return exposed === bombCount;
    }

    let surrGrid = bombGrid.map((v, i) => {
        return v.map((w, j) => {
            return calcSurround(i, j);
        });
    });

    let clicked = 0;
    let score = null;

    onMount(() => {
        bombCount = 0;
        exposed = 0;
        clicked = 0;
        startTime = new Date();

        bombGrid = Array.from({length: rows}, (v, num) => {
            return Array.from({length: columns}, (val2, num2) => {
                let r = Math.random();
                if (r < chance) bombCount++;
                return r < chance;
            });
        });

        surrGrid = bombGrid.map((v, i) => {
            return v.map((w, j) => {
                return calcSurround(i, j);
            });
        });

        let expoGrid = bombGrid.map((v, i) => {
            return v.map((w, j) => {
                return null;
            });
        });
    });
</script>

<div class="m-cont">
    <Score bind:clicks={clicked} bind:this={score}/>
    <div
            class="m-grid"
            style="grid-template-rows: repeat({rows}, {size}); grid-template-columns: repeat({columns}, {size}); aspect-ratio: {rows / columns};"

    >
        {#each bombGrid as row, x}
            {#each row as cell, y}
                <MCell
                        row={x}
                        col={y}
                        isBomb={cell}
                        bind:this={expoGrid[x][y]}
                        surroundingCount={surrGrid[x][y]}
                        on:pressed|once={(e) => {
            clicked++;
          }}
                        on:expose|once={(e) => {
            if (e.detail.isBomb) {
              dispatch("lose");
              return;
            }
            exposed++;
            if (columns * rows - exposed === bombCount) {
              console.log(score.currentTime);
              dispatch("win", {
                clicked,
                time:
                  score.getTime(),
              });
              return;
            }
            if (e.detail.surroundingCount === 0) {
              exposeSurr(e.detail.row, e.detail.col);
            }
          }}
                />
            {/each}
        {/each}
    </div>
</div>

<style>
    .m-grid {
        font-family: "Press Start 2P", cursive;
        font-size: 0.8em;
        display: grid;
        width: min(80vw, 80vh);
        height: min(80vw, 80vh);
        border: 0.5em inset #adadad;
        border-bottom: inset 0.5em #ffffff;
        border-right: inset 0.5em #ffffff;
    }

    .m-cont {
        display: grid;
        place-items: center;
        grid-template-rows: 1fr 9fr;
        grid-template-columns: 1fr;
        background-color: #c0c0c0;
        padding: 1em;

        border: 0.5em outset #adadad;
        border-top: outset 0.5em #ececec;
        border-left: outset 0.5em #ececec;
    }
</style>
