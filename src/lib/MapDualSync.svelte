<script>
	/** breadcrumbs
	 * https://github.com/mapbox/mapbox-gl-compare/issues/1
	 *
	 */
	import { onMount, onDestroy } from 'svelte'
	import { Map } from 'mapbox-gl'
	// import Compare from 'mapbox-gl-compare'
	import SyncMaps from '@mapbox/mapbox-gl-sync-move'
	import '../../node_modules/mapbox-gl/dist/mapbox-gl.css'
	// import '../../node_modules/mapbox-gl-compare/dist/mapbox-gl-compare.css'

	let mapA, mapB, syncMap
	let mapContainerA, mapContainerB, swipeContainer
	export let config = {
		lng: 13.06,
		lat: 52.4006,
		zoom: 14
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

		syncMap = new SyncMaps(mapA, mapB)
	})

	$: position = syncMap?.currentPosition
	onDestroy(() => {
		mapA.remove()
		mapB.remove()
		// syncMap.off()
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
		width: 50%;
	}
	.map:nth-of-type(2n) {
		right: 0;
	}

	/* .map {
		position: relative;
		height: 100vh;
		z-index: -1;
	} */
</style>
