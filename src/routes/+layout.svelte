<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<style>
		body {
			margin: 0;
			font-family: 'Inter', sans-serif;
			color: #19191e;
			background: #f7f7f9;
		}

		a {
			color: inherit;
		}
	</style>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}
