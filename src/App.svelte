<script>
	import MapSimple from '$lib/MapSimple.svelte'
	import MapDualSwipe from '$lib/MapDualSwipe.svelte'
	import MapDualSync from '$lib/MapDualSync.svelte'
	let isSingle = false
	let isSwipe = false

	const toggleView = () => {
		isSingle = !isSingle
	}

	const toggleSwipe = () => {
		isSwipe = !isSwipe
	}
</script>

<main>
	<aside>
		<button on:click={toggleView}>Toggle View</button>
		{#if !isSingle}
			<button on:click={toggleSwipe}>Toggle Swipe ({isSwipe ? 'on' : 'off'})</button>
		{/if}
	</aside>
	{#if isSingle}
		<MapSimple />
	{:else if isSwipe}
		<MapDualSwipe />
	{:else}
		<MapDualSync />
	{/if}
</main>

<style>
	main {
		/* position: relative; */
		width: 100vw;
		height: 100vh;
		background-color: aquamarine;
	}

	aside {
		position: absolute;
		top: 0;
		left: 0;
		z-index: 100;
	}
</style>
