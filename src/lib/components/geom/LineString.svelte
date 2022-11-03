<script lang="ts">
	import { Feature } from 'ol';
	import type { Geometry } from 'ol/geom';
	import { LineString } from 'ol/geom';
	import type VectorLayer from 'ol/layer/Vector';
	import type VectorSource from 'ol/source/Vector';
	import { getContext, onMount } from 'svelte';

	export let coords: [number, number][];

	const getVectorLayer = getContext<() => VectorLayer<VectorSource<Geometry>>>('vectorLayer');

	onMount(() => {
		const vectorLayer = getVectorLayer();
		const vectorSource = vectorLayer.getSource();

		const line = new LineString(coords);

		const lineFeature = new Feature(line);

		vectorSource?.addFeature(lineFeature);
	});
</script>
