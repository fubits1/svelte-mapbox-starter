<script>
	import { onMount, onDestroy } from 'svelte'
	import { Map } from 'mapbox-gl'
	import Compare from 'mapbox-gl-compare'
	import '../../node_modules/mapbox-gl/dist/mapbox-gl.css'
	import '../../node_modules/mapbox-gl-compare/dist/mapbox-gl-compare.css'

	let mapA, mapB, swipeMap
	let mapContainerA, mapContainerB, swipeContainer
	export let config = {
		lng: 13.06,
		lat: 52.4006,
		zoom: 12
	}

	const initialState = { lng: config.lng, lat: config.lat, zoom: config.zoom }

	onMount(() => {
		mapA = new Map({
			container: mapContainerA,
			accessToken: import.meta.env.VITE_MAPBOX_PUBLIC_TOKEN,
			style: `mapbox://styles/mapbox/light-v11`,
			center: [initialState.lng, initialState.lat],
			zoom: initialState.zoom
		})

		mapB = new Map({
			container: mapContainerB,
			accessToken: import.meta.env.VITE_MAPBOX_PUBLIC_TOKEN,
			style: `mapbox://styles/mapbox/dark-v11`,
			center: [initialState.lng, initialState.lat],
			zoom: initialState.zoom
		})

		swipeMap = new Compare(mapA, mapB, swipeContainer, {
			orientation: 'vertical'
		})

		swipeMap.setSlider(300)
	})

	$: position = swipeMap?.currentPosition
	onDestroy(() => {
		mapA.remove()
		mapB.remove()
		swipeMap.remove()
	})
</script>

<section>
	<code style="position: absolute; right: 0; z-index: 999;">{position}</code>
	<div class="map-wrap">
		<div id="comparison-container" bind:this={swipeContainer}>
			<div id="before" class="map" bind:this={mapContainerA}></div>
			<div id="after" class="map" bind:this={mapContainerB}></div>
		</div>
	</div>
</section>

<style>
	#comparison-container {
		position: relative;
		/* width: 100vw; */
		height: 100vh;
	}

	.map {
		position: absolute;
		height: 100%;
		width: 100%;
	}
</style>
