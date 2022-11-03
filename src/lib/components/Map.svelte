<script lang="ts">
	import { Map, View } from 'ol';
	import TileLayer from 'ol/layer/Tile';
	import { transform } from 'ol/proj';
	import OSM from 'ol/source/OSM';
	import { onMount, setContext } from 'svelte';

	let viewDiv = 'openlayersmap';
	let map: Map;

	const center = transform([5.702586, 46.018994], 'EPSG:4326', 'EPSG:3857');

	setContext('map', () => map);

	onMount(() => {
		map = new Map({
			target: viewDiv,
			view: new View({
				center,
				zoom: 12
			}),
			layers: [
				new TileLayer({
					source: new OSM()
				})
			]
		});
	});
</script>

<div id={viewDiv} class="map" />

{#if map}
	<slot />
{/if}

<style>
	.map {
		width: 100%;
		height: 100%;
	}
</style>
