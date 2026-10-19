<script>
  // Constants
  const winCombos = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8], // Rows
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8], // Cols
    [0, 4, 8],
    [2, 4, 6], // Diagonals
  ];

  // State
  let cells = $state(Array(9).fill(""));
  let turn = $state("X");

  // Derived
  let winLine = $derived.by(() => {
    for (let [a, b, c] of winCombos) {
      if (cells[a] !== "" && cells[a] === cells[b] && cells[a] === cells[c]) {
        return [a, b, c]; // Return the 3 winnin row or column
      }
    }
    return [];
  });

  let winner = $derived(winLine.length > 0 ? cells[winLine[0]] : "");
  let isDraw = $derived(winner === "" && !cells.includes(""));

  // Functions
  function play(i) {
    if (cells[i] !== "" || winner !== "") return;
    cells[i] = turn;
    turn = turn === "X" ? "O" : "X";
  }

  function reset() {
    cells.fill("");
    turn = "X";
  }
</script>

<main
  class="min-h-screen flex flex-col items-center justify-evenly p-5 bg-zinc-900 text-zinc-100"
>
  <section class="p-2">
    <h1 class="text-5xl font-extrabold pb-3 border-b-2 border-b-zinc-600">
      Tic Tac Toe
    </h1>
  </section>

  <section
    class="border-2 border-zinc-400 h-[clamp(30dvh,40dvh,80dvh)] aspect-square grid grid-cols-3 grid-rows-3"
  >
    {#each cells as cell, i}
      {@const isWinTile = winLine.includes(i)}
      <button
        onclick={() => play(i)}
        class="border border-zinc-700 text-5xl font-extrabold transition duration-300 hover:bg-zinc-800
          {cell === 'X' ? 'text-rose-400' : 'text-cyan-400'}
          {isWinTile ? 'scale-110 animate-ping bg-zinc-800 z-10 ]' : ''}"
      >
        {cell}
      </button>
    {/each}
  </section>

  <div class="h-10 flex items-center">
    {#if winner}
      <div
        class="bg-zinc-800 px-6 py-2 rounded font-bold text-xl"
        class:text-rose-400={winner === "X"}
        class:text-cyan-400={winner === "O"}
      >
        Player {winner} Won!
      </div>
    {:else if isDraw}
      <div
        class="bg-zinc-800 px-6 py-2 rounded font-bold text-xl text-zinc-300"
      >
        Draw!
      </div>
    {:else}
      <div class="text-zinc-400 text-2xl font-semibold">
        Turn : <span class={turn === "X" ? "text-rose-400" : "text-cyan-400"}
          >{turn}</span
        >
      </div>
    {/if}
  </div>

  <button
    onclick={reset}
    class="px-6 py-2 bg-zinc-100 text-zinc-900 font-bold rounded-lg hover:bg-zinc-300 transition active:scale-95"
  >
    Reset
  </button>
</main>
