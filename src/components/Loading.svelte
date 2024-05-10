<script lang="ts">
	import { browser } from '$app/environment';
	import { createEventDispatcher, onMount } from 'svelte';

	// Event Dispatch
	const dispatch = createEventDispatcher();

	/* Word Animation */
	let words: String[] = [
		'Hello',
		'નમસ્તે',
		'CIAO',
		'こんにちは',
		'Hola',
		'안녕',
		'Bonjour',
		'Olá',
		'Hallo',
		'你好',
		'नमस्ते'
	];

	let index: number = 0;
	let word: String = words[index];
	let intervalDuration: number = 1000;

	const switchWord = () => {
		intervalDuration = 400 - index * 10;
		clearInterval(run);

		if (index >= words.length - 1) {
			if (browser) {
				const cum = document.getElementById('bar');
				cum?.classList.remove('hidden');
				cum?.classList.add('stretch');

				setTimeout(() => dispatch('close'), 1800);
			}
		} else index++;

		word = words[index];
		run = setInterval(switchWord, intervalDuration);
	};

	/* Rain Animation */
	const rain = () => {
		if (browser) {
			let increment = 0;
			const rainElement = document.getElementById('rain');

			while (increment < 95) {
				let randoHundo = Math.floor(Math.random() * (98 - 1 + 1) + 1);
				let randoFiver = Math.floor(Math.random() * (4 - 2 + 1) + 2);
				increment += randoFiver;
				if (rainElement)
					rainElement.innerHTML +=
						'<div class="drop" style="left: ' +
						increment +
						'%; bottom: ' +
						(randoFiver + randoFiver - 1 + 100) +
						'%; animation-delay: 0.' +
						randoHundo +
						's; animation-duration: 0.5' +
						randoHundo +
						's;"><div class="stem" style="animation-delay: 0.' +
						randoHundo +
						's; animation-duration: 0.5' +
						randoHundo +
						's;"></div><div class="splat" style="animation-delay: 0.' +
						randoHundo +
						's; animation-duration: 0.5' +
						randoHundo +
						's;"></div></div>';
			}
		}
	};

    onMount(rain);
	let run = setInterval(switchWord, intervalDuration);
</script>

<div id="rain"></div>

<div class="grid place-items-center min-h-screen overflow-none no-scrollbar">
	<ol class="list-disc no-scrollbar">
		<li
			class="animate__animated animate__bounce animate__infinite animate__fast text-warning-600 font-extrabold tracking-tight italic text-5xl no-scrollbar"
		>
			{word}
		</li>
	</ol>
</div>

<div id="bar" class="hidden bg-gray-800 p-4" />
