<script lang="ts">
	type Note = 'a' | 'b' | 'c' | 'd' | 'e' | 'f' | 'g';
	type Type = 'treble' | 'bass';
	type Question = { type: Type; note: Note; top: string };
	const questions: Question[] = [
		{ type: 'treble', note: 'f', top: '-19px' },
		{ type: 'treble', note: 'e', top: '-11px' },
		{ type: 'treble', note: 'd', top: '-2px' },
		{ type: 'treble', note: 'c', top: '6px' },
		{ type: 'treble', note: 'b', top: '15px' },
		{ type: 'treble', note: 'a', top: '23px' },
		{ type: 'treble', note: 'g', top: '32px' },
		{ type: 'treble', note: 'f', top: '40px' },
		{ type: 'treble', note: 'e', top: '49px' },
		{ type: 'treble', note: 'd', top: '57px' },
		{ type: 'treble', note: 'c', top: '66px' },
		{ type: 'treble', note: 'b', top: '74px' },
		{ type: 'treble', note: 'a', top: '83px' },
		{ type: 'treble', note: 'g', top: '91px' },
		{ type: 'treble', note: 'f', top: '100px' },
		{ type: 'treble', note: 'e', top: '108px' },
		{ type: 'treble', note: 'd', top: '117px' },
		{ type: 'treble', note: 'c', top: '125px' },
		{ type: 'treble', note: 'b', top: '134px' },
		{ type: 'treble', note: 'a', top: '142px' },
		{ type: 'treble', note: 'g', top: '151px' },
		{ type: 'treble', note: 'f', top: '159px' },
		{ type: 'treble', note: 'e', top: '169px' },

		{ type: 'bass', note: 'a', top: '-19px' },
		{ type: 'bass', note: 'g', top: '-11px' },
		{ type: 'bass', note: 'f', top: '-2px' },
		{ type: 'bass', note: 'e', top: '6px' },
		{ type: 'bass', note: 'd', top: '15px' },
		{ type: 'bass', note: 'c', top: '23px' },
		{ type: 'bass', note: 'b', top: '32px' },
		{ type: 'bass', note: 'a', top: '40px' },
		{ type: 'bass', note: 'g', top: '49px' },
		{ type: 'bass', note: 'f', top: '57px' },
		{ type: 'bass', note: 'e', top: '66px' },
		{ type: 'bass', note: 'd', top: '74px' },
		{ type: 'bass', note: 'c', top: '83px' },
		{ type: 'bass', note: 'b', top: '91px' },
		{ type: 'bass', note: 'a', top: '100px' },
		{ type: 'bass', note: 'g', top: '108px' },
		{ type: 'bass', note: 'f', top: '117px' },
		{ type: 'bass', note: 'e', top: '125px' },
		{ type: 'bass', note: 'd', top: '134px' },
		{ type: 'bass', note: 'c', top: '142px' },
		{ type: 'bass', note: 'b', top: '151px' },
		{ type: 'bass', note: 'a', top: '159px' },
		{ type: 'bass', note: 'g', top: '168px' }
	];

	let question = $state(getNextQuestion(questions));
	let answersIncorrect = $state(0);
	let answersCorrect = $state(0);
	let answersTotal = $derived(answersIncorrect + answersCorrect);
	let lastAnswer = $state<boolean | null>(null);

	function getRandomInt(max: number) {
		return Math.floor(Math.random() * max);
	}

	function getNextQuestion(notes: Question[]) {
		return notes[getRandomInt(notes.length)];
	}

	function answer(answer: Note) {
		lastAnswer = answer === question.note;
		if (lastAnswer) {
			answersCorrect++;
			question = getNextQuestion(questions);
		} else {
			answersIncorrect++;
		}
	}

	function showTopBar(index: number, type: Type) {
		const qIndex = questions
			.filter((q) => q.type === type)
			.findIndex((q) => JSON.stringify(q) === JSON.stringify(question));

		return qIndex > -1 && qIndex <= index;
	}

	function showBottomBar(index: number, type: Type) {
		const filtered = questions.filter((q) => q.type === type);
		const qIndex = filtered.findIndex((q) => JSON.stringify(q) === JSON.stringify(question));

		return qIndex > -1 && filtered.length - qIndex <= index;
	}
</script>

<svelte:head>
	<title>Quick Note Practice</title>
</svelte:head>

<main
	class="w-100 mx-auto mt-14 flex max-w-screen-md flex-col gap-14 text-slate-800 dark:text-white"
>
	<div class="relative flex flex-col" style="gap: 15px;">
		<div
			style="font-size: 170px; line-height: 170px; top:-17px;"
			class="absolute left-0 top-0 dark:text-white"
		>
			𝄞
		</div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showTopBar(1, 'treble') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showTopBar(3, 'treble') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showTopBar(5, 'treble') && 'border-black dark:border-white'
			]}
		></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showBottomBar(6, 'treble') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showBottomBar(4, 'treble') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showBottomBar(2, 'treble') && 'border-black dark:border-white'
			]}
		></div>
		{#if question.type === 'treble'}
			<div
				class="whole-note absolute dark:border-white"
				style="left:50%; top:-19px;"
				style:top={question.top}
			></div>
		{/if}
	</div>
	<div class="relative flex flex-col" style="gap: 15px;">
		<div
			style="font-size: 85px; line-height: 85px; top:33px; left:5px;"
			class="absolute dark:text-white"
		>
			𝄢
		</div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showTopBar(1, 'bass') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showTopBar(3, 'bass') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showTopBar(5, 'bass') && 'border-black dark:border-white'
			]}
		></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div class="border border-solid border-black dark:border-white"></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showBottomBar(6, 'bass') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showBottomBar(4, 'bass') && 'border-black dark:border-white'
			]}
		></div>
		<div
			class={[
				'mx-auto w-7 border border-solid border-transparent',
				showBottomBar(2, 'bass') && 'border-black dark:border-white'
			]}
		></div>
		{#if question.type === 'bass'}
			<div
				class="whole-note absolute border-black dark:border-white"
				style="left:50%; top:-19px;"
				style:top={question.top}
			></div>
		{/if}
	</div>

	<div class="flex justify-center">
		<span class={{ 'text-green-500': lastAnswer, 'text-red-500': lastAnswer === false }}
			>{answersCorrect} / {answersTotal}</span
		>
	</div>

	<div class="flex justify-center gap-2">
		<button
			type="button"
			class="rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('a')}>a</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('b')}>b</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('c')}>c</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('d')}>d</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('e')}>e</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('f')}>f</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('g')}>g</button
		>
	</div>
</main>

<style>
	.whole-note {
		left: 50%;
		width: 25px;
		height: 15px;
		background-color: transparent;
		border-width: 3px;
		border-style: solid;
		border-radius: 50%;
		transform: rotate(-20deg) translateX(-50%); /* Gives it the characteristic tilt of musical notation */
	}
</style>
