<script>
  let cells = $state(Array(9).fill(""));
  let turn = $state("X");
  let isDraw = $derived(!cells.includes("") && winner === "");

  const reset = () => {
    cells.fill("");
    turn = "X";
  };

  const winCombos = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 8],
  ];

  function play(i) {
    if (cells[i] !== "") return;
    if (winner !== "") return;

    cells[i] = turn;
    turn = turn === "X" ? "O" : "X";
  }

  let winner = $derived.by(() => {
    for (let [a, b, c] of winCombos) {
      if (cells[a] !== "" && cells[a] === cells[b] && cells[a] === cells[c]) {
        return cells[a];
      }
    }

    return "";
  });
</script>

<main
  class="min-h-screen flex flex-col items-center justify-evenly p-5 bg-zinc-900 text-zinc-100"
>
  <section class="mt-2 p-4">
    <h1 class="text-5xl font-extrabold pb-3 border-b-2 border-b-zinc-600">
      Tic Tac Toe
    </h1>
  </section>
  <section
    class="border-2 border-zinc-400 h-[clamp(30dvh,20vw,80dvh)] aspect-square grid grid-cols-3 grid-rows-3"
  >
    {#each cells as cell, i}
      <button
        onclick={() => play(i)}
        class="border border-zinc-700 text-5xl font-extrabold transition hover:bg-zinc-800
          {cell === 'X' ? 'text-rose-400' : 'text-cyan-400'}
          {winner !== '' ? 'animate-spin' : ''}
          "
      >
        {cell}</button
      >
    {/each}
  </section>
  {#if winner}
    <div
      class="bg-zinc-800 px-6 py-2"
      class:text-rose-500={winner === "X"}
      class:text-green-500={winner === "O"}
    >
      Player {winner} Won! Player {winner} Won!
    </div>
  {:else if isDraw}
    Draw
  {/if}
  <button
    onclick={reset}
    class="px-6 py-2 bg-zinc-100 text-zinc-900 font-bold rounded-lg hover:bg-zinc-300"
  >
    reset
  </button>
</main>
