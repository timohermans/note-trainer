<script lang="ts">
	const { pressed } = $props();
	const NOTE_NAMES = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];

	function getMidiNoteName(midiNumber: number): string {
		const octave = Math.floor(midiNumber / 12) - 1;
		const noteIndex = midiNumber % 12;
		return `${NOTE_NAMES[noteIndex]}${octave}`;
	}

	// Piano key data
	const keys = Array.from({ length: 88 }, (_, i) => {
		const noteNumber = i + 21; // MIDI notes start at 21 (A0)
		const isBlack = [1, 3, 6, 8, 10].includes(noteNumber % 12);
		return { noteNumber, isBlack };
	});

	function handleKeyPress(noteNumber: number) {
		pressed(noteNumber);
	}
</script>

<div class="piano-container">
	<div class="piano">
		{#each keys as { noteNumber, isBlack }, index}
			{#if !isBlack}
				<div class="key-container">
					<button
						style="writing-mode: vertical-rl;"
						class="key {isBlack ? 'black' : 'white'}"
						onmousedown={() => handleKeyPress(noteNumber)}
						aria-label="Piano key {noteNumber}"
					></button>

					{#if index != keys.length - 1 && keys[index + 1].isBlack}
						<button
							class="key {keys[index + 1].isBlack ? 'black' : 'white'}"
							onmousedown={() => handleKeyPress(keys[index + 1].noteNumber)}
							aria-label="Piano key {keys[index + 1].noteNumber}"
						></button>
					{/if}
				</div>
			{/if}
		{/each}
	</div>
</div>

<style>
	.piano-container {
		width: 100%;
		height: 125px;
		padding: 4px;
		background: #333;
		border-radius: 8px;
	}

	.piano {
		position: relative;
		height: 100%;
		display: flex;
	}

	.key-container {
		position: relative;
		border: 1px solid #000;
		padding: 0;
		margin: 0;
		flex: 1;
		transition: background-color 0.1s;
	}

	.key {
		width: 100%;
	}

	.white {
		background: white;
		height: 100%;
		z-index: 1;
		border-bottom-left-radius: 4px;
		border-bottom-right-radius: 4px;
	}

	.black {
		background: black;
		height: 60%;
		width: 70%;
		position: absolute;
		z-index: 2;
		transform: translateX(-50%);
	}

	.white:active {
		background: #eee;
	}

	.black:active {
		background: #333;
	}

	/* Ensure the last few white keys don't get covered by black keys */
	.white:nth-last-child(-n + 3) {
		z-index: 1;
	}
</style>
