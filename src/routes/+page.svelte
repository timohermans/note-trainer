<script lang="ts">
	import PianoKeys from '$lib/PianoKeys.svelte';
	import { onMount } from 'svelte';
	import { on } from 'svelte/events';

	const isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);

	onMount(() => {
		on(window, 'orientationchange', () => {
			orientation = window.screen.orientation.type;
		});
		orientation = window.screen.orientation.type;
	});

	type Note = 'a' | 'b' | 'c' | 'd' | 'e' | 'f' | 'g';
	type Type = 'treble' | 'bass';
	type Question = { type: Type; number: number; note: Note; top: string };
	const questions: Question[] = [
		{ type: 'treble', number: 89, note: 'f', top: '-19px' },
		{ type: 'treble', number: 88, note: 'e', top: '-11px' },
		{ type: 'treble', number: 86, note: 'd', top: '-2px' },
		{ type: 'treble', number: 84, note: 'c', top: '6px' },
		{ type: 'treble', number: 83, note: 'b', top: '15px' },
		{ type: 'treble', number: 81, note: 'a', top: '23px' },
		{ type: 'treble', number: 79, note: 'g', top: '32px' },
		{ type: 'treble', number: 77, note: 'f', top: '40px' },
		{ type: 'treble', number: 76, note: 'e', top: '49px' },
		{ type: 'treble', number: 74, note: 'd', top: '57px' },
		{ type: 'treble', number: 72, note: 'c', top: '66px' },
		{ type: 'treble', number: 71, note: 'b', top: '74px' },
		{ type: 'treble', number: 69, note: 'a', top: '83px' },
		{ type: 'treble', number: 67, note: 'g', top: '91px' },
		{ type: 'treble', number: 65, note: 'f', top: '100px' },
		{ type: 'treble', number: 64, note: 'e', top: '108px' },
		{ type: 'treble', number: 62, note: 'd', top: '117px' },
		{ type: 'treble', number: 60, note: 'c', top: '125px' },
		{ type: 'treble', number: 59, note: 'b', top: '134px' },
		{ type: 'treble', number: 57, note: 'a', top: '142px' },
		{ type: 'treble', number: 55, note: 'g', top: '151px' },
		{ type: 'treble', number: 53, note: 'f', top: '159px' },
		{ type: 'treble', number: 52, note: 'e', top: '169px' },

		{ type: 'bass', number: 69, note: 'a', top: '-19px' },
		{ type: 'bass', number: 67, note: 'g', top: '-11px' },
		{ type: 'bass', number: 65, note: 'f', top: '-2px' },
		{ type: 'bass', number: 64, note: 'e', top: '6px' },
		{ type: 'bass', number: 62, note: 'd', top: '15px' },
		{ type: 'bass', number: 60, note: 'c', top: '23px' },
		{ type: 'bass', number: 59, note: 'b', top: '32px' },
		{ type: 'bass', number: 57, note: 'a', top: '40px' },
		{ type: 'bass', number: 55, note: 'g', top: '49px' },
		{ type: 'bass', number: 53, note: 'f', top: '57px' },
		{ type: 'bass', number: 52, note: 'e', top: '66px' },
		{ type: 'bass', number: 50, note: 'd', top: '74px' },
		{ type: 'bass', number: 48, note: 'c', top: '83px' },
		{ type: 'bass', number: 47, note: 'b', top: '91px' },
		{ type: 'bass', number: 45, note: 'a', top: '100px' },
		{ type: 'bass', number: 43, note: 'g', top: '108px' },
		{ type: 'bass', number: 41, note: 'f', top: '117px' },
		{ type: 'bass', number: 40, note: 'e', top: '125px' },
		{ type: 'bass', number: 38, note: 'd', top: '134px' },
		{ type: 'bass', number: 36, note: 'c', top: '142px' },
		{ type: 'bass', number: 35, note: 'b', top: '151px' },
		{ type: 'bass', number: 33, note: 'a', top: '159px' },
		{ type: 'bass', number: 31, note: 'g', top: '168px' }
	];

	let orientation: OrientationType = $state('landscape-primary');
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

	function answer(answer: Note | number) {
		lastAnswer = answer === question.note || answer === question.number;
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

<div class="flex h-screen flex-col justify-between">
	<main
		class="flex w-full max-w-screen-md flex-col gap-8 self-center pt-5 text-slate-800 dark:text-white"
	>
		{#if !isMobile || (isMobile && orientation === 'landscape-primary' && question.type === 'treble') || (isMobile && orientation == 'portrait-primary')}
			<div class="relative flex flex-col" style="gap: 15px;">
				<div
					style="font-size: 170px; line-height: 170px; top:-17px;"
					class="absolute left-0 top-0 dark:text-white"
				>
					𝄞
				</div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showTopBar(1, 'treble') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showTopBar(3, 'treble') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showTopBar(5, 'treble') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showBottomBar(6, 'treble') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showBottomBar(4, 'treble') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showBottomBar(2, 'treble') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				{#if question.type === 'treble'}
					<div
						class="whole-note absolute border-black dark:border-white"
						style="left:50%; top:-19px;"
						style:top={question.top}
					></div>
				{/if}
			</div>
		{/if}

		{#if !isMobile || (isMobile && orientation === 'landscape-primary' && question.type === 'bass') || (isMobile && orientation == 'portrait-primary')}
			<div class="relative flex flex-col" style="gap: 15px;">
				<div
					style="font-size: 85px; line-height: 85px; top:33px; left:5px;"
					class="absolute dark:text-white"
				>
					𝄢
				</div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showTopBar(1, 'bass') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showTopBar(3, 'bass') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showTopBar(5, 'bass') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div class="border border-solid border-black dark:border-white"></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showBottomBar(6, 'bass') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showBottomBar(4, 'bass') ? 'border-black dark:border-white' : 'border-transparent'
					]}
				></div>
				<div
					class={[
						'mx-auto w-7 border border-solid',
						showBottomBar(2, 'bass') ? 'border-black dark:border-white' : 'border-transparent'
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
		{/if}

		<div class="flex justify-center">
			<span class={{ 'text-green-500': lastAnswer, 'text-red-500': lastAnswer === false }}
				>{answersCorrect} / {answersTotal}</span
			>
		</div>
	</main>

	<div class="flex justify-center gap-2 landscape:hidden pb-28">
		<button
			type="button"
			class="rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('a')}>a</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 hover:text-white dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('b')}>b</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 hover:text-white dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('c')}>c</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 hover:text-white dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('d')}>d</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 hover:text-white dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('e')}>e</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 hover:text-white dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('f')}>f</button
		>
		<button
			type="button"
			class=" rounded border border-solid border-slate-800 p-4 text-slate-800 hover:bg-slate-700 hover:text-white dark:border-white dark:text-white dark:hover:bg-slate-700"
			onclick={() => answer('g')}>g</button
		>
	</div>

	<div class="flex justify-center portrait:hidden">
		<PianoKeys pressed={answer} />
	</div>
</div>

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
