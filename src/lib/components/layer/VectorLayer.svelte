<script lang="ts">
	import type { Map } from 'ol';
	import type { Geometry } from 'ol/geom';
	import VectorLayer from 'ol/layer/Vector';
	import VectorSource from 'ol/source/Vector';
	import { getContext, onMount, setContext } from 'svelte';

	const getMap = getContext<() => Map>('map');
	let vectorLayer: VectorLayer<VectorSource<Geometry>>;

	setContext('vectorLayer', () => vectorLayer);

	onMount(() => {
		const map = getMap();
		vectorLayer = new VectorLayer();
		const vectorSource = new VectorSource();
		vectorLayer.setSource(vectorSource);

		map?.addLayer(vectorLayer);
	});
</script>

{#if vectorLayer}
	<slot />
{/if}
