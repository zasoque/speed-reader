<script lang="ts">
	import { onMount } from 'svelte';

	let text = $state('');
	let reading = $state(false);
	let speed = $state(15);

	let word = $state('');

	async function read() {
		reading = true;

		const words = text.replace('(', ' (').replace(')', ') ').split(/\s+/);
		for (const w of words) {
			word = w;
			const period = w.endsWith('.') || w.endsWith('!') || w.endsWith('?');
			if (reading) {
				await new Promise((resolve) => setTimeout(resolve, w.length * speed * (period ? 1.5 : 1)));
			}
		}

		reading = false;
	}

	onMount(() => {
		const keydown = (event) => {
			reading = false;
		};

		document.addEventListener('keydown', keydown);

		return () => {
			document.removeEventListener('keydown', keydown);
		};
	});
</script>

<div class="container">
	{#if reading}
		<div class="reader">
			<div class="reader-left">{word.substring(0, word.length / 2)}</div>
			<div class="reader-right">{word.substring(word.length / 2)}</div>
		</div>
	{:else}
		<textarea bind:value={text}></textarea>
		<input type="range" min="10" max="100" bind:value={speed} />
		<div>
			<button onclick={read}>READ</button>
		</div>
	{/if}
</div>

<style>
	.container {
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		max-width: 960px;
		margin: 0 auto;
	}

	textarea {
		width: 100%;
		height: 200px;
		color: #282828;
		border: none;
		border-radius: 4px;
		padding: 10px;
		background-color: white;
		font-size: 16px;
		resize: none;
		box-sizing: border-box;
	}

	button {
		margin-top: 10px;
		padding: 10px 20px;
		color: #3f3f3f;
		border: none;
		border-radius: 4px;
		background-color: white;
		font-size: 16px;
		cursor: pointer;
	}

	.reader {
		font-size: 48px;
		display: flex;
		flex-direction: row;
		justify-content: center;
	}

	.reader-left {
		flex: 1;
		text-align: right;
	}

	.reader-right {
		flex: 1;
		text-align: left;
	}
</style>
