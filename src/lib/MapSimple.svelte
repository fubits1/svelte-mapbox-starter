<script>
	import { onMount, onDestroy } from 'svelte'
	import { Map } from 'mapbox-gl'
	import '../../node_modules/mapbox-gl/dist/mapbox-gl.css'

	let map
	let mapContainer
	let lng, lat, zoom

	lng = 13.06
	lat = 52.4006
	zoom = 12

	onMount(() => {
		const initialState = { lng: lng, lat: lat, zoom: zoom }

		map = new Map({
			container: mapContainer,
			accessToken: import.meta.env.VITE_MAPBOX_PUBLIC_TOKEN,
			style: `mapbox://styles/mapbox/outdoors-v11`,
			center: [initialState.lng, initialState.lat],
			zoom: initialState.zoom
		})
	})

	onDestroy(() => {
		map.remove()
	})
</script>

<main>
	<div class="map-wrap">
		<div class="map" bind:this={mapContainer} />
	</div>
</main>

<style>
	.map {
		position: absolute;
		width: 100%;
		height: 100%;
	}
</style>
