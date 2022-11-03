<script lang="ts">
	import { Feature } from 'ol';
	import type { Geometry } from 'ol/geom';
	import { Point } from 'ol/geom';
	import type VectorLayer from 'ol/layer/Vector';
	import type VectorSource from 'ol/source/Vector';
	import { getContext, onMount } from 'svelte';

	export let coords: number[];

	const getVectorLayer = getContext<() => VectorLayer<VectorSource<Geometry>>>('vectorLayer');

	onMount(() => {
		const vectorLayer = getVectorLayer();
		const vectorSource = vectorLayer.getSource();

		const point = new Point(coords);
		const feature = new Feature(point);

		vectorSource?.addFeature(feature);
	});
</script>
