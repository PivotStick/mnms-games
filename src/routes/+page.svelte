<script>
	import '@fortawesome/fontawesome-free/css/all.min.css';
	import '../app.scss';

	let turnIndex = 0;
	let players = ['X', 'O'];
	/**
	 * @type {Record<string, number>}
	 */
	let winnerHistory = {};
	/**
	 * @type {null | string}
	 */
	let winner = null;
	let draw = false;

	/**
	 * @type {(null | string)[][]}
	 */
	let grid = [
		[null, null, null],
		[null, null, null],
		[null, null, null]
	];

	let initialGrid = JSON.stringify(grid);

	/**
	 * @param {number} rowIndex
	 * @param {number} colIndex
	 */
	function hasCellWon(rowIndex, colIndex) {
		return (
			checkLine(colIndex, rowIndex, 1, 0) ||
			checkLine(colIndex, rowIndex, 1, 1) ||
			checkLine(colIndex, rowIndex, 0, 1) ||
			checkLine(colIndex, rowIndex, -1, 1) ||
			checkLine(colIndex, rowIndex, -1, 0) ||
			checkLine(colIndex, rowIndex, -1, -1) ||
			checkLine(colIndex, rowIndex, 0, -1)
		);
	}

	/**
	 * @param {number} x
	 * @param {number} y
	 * @param {number} dirX
	 * @param {number} dirY
	 */
	function checkLine(x, y, dirX, dirY) {
		let value = grid[y][x];

		for (let i = 0; i < 2; i++) {
			x += dirX;
			y += dirY;

			if (value !== grid?.[y]?.[x]) {
				return false;
			}
		}

		return true;
	}

	function getWinner() {
		let isFull = true;

		for (let y = 0; y < grid.length; y++) {
			const row = grid[y];
			for (let x = 0; x < row.length; x++) {
				if (grid[y][x]) {
					if (hasCellWon(y, x)) {
						return { winner: grid[y][x], isFull };
					}
				} else {
					isFull = false;
				}
			}
		}

		return { winner: null, isFull };
	}
</script>

<main>
	<pre>{JSON.stringify(winnerHistory, null, 2)}</pre>
	{#if winner || draw}
		{#if draw}
			DRAW wtfff
		{:else}
			{winner} os.remove()
		{/if}
		<button
			on:click={() => {
				winner = null;
				draw = false;
				turnIndex = 0;
				grid = JSON.parse(initialGrid);
			}}
		>
			Replay <i class="fa fa-refresh"></i>
		</button>
	{:else}
		<h1>{players[turnIndex]}'s turn</h1>
	{/if}

	<div class="grid">
		{#each grid as row, rowIndex}
			{#each row as cell, colIndex}
				<div
					class="cell"
					on:click={() => {
						if (winner || draw || grid[rowIndex][colIndex] !== null) return;

						grid[rowIndex][colIndex] = players[turnIndex];
						const results = getWinner();

						winner = results.winner;

						if (!winner) {
							if (results.isFull) {
								draw = true;
							} else {
								turnIndex = (turnIndex + 1) % players.length;
							}
						} else {
							winnerHistory[winner] = (winnerHistory[winner] || 0) + 1;
						}
					}}
				>
					{cell ?? ''}
				</div>
			{/each}
		{/each}
	</div>
</main>

<style lang="scss">
	main {
		height: 100vh;
		display: flex;
		flex-direction: column;
		gap: 3rem;
		justify-content: center;
		align-items: center;
	}

	.grid {
		$size: 6rem;

		display: grid;
		grid-template-columns: repeat(3, $size);
		grid-template-rows: repeat(3, $size);
		width: max-content;
		border: 1px solid black;

		.cell {
			display: flex;
			align-items: center;
			justify-content: center;
			font-size: calc($size / 2);

			border: 1px solid black;
		}
	}
</style>
