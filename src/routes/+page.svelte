<script lang="ts">
	import LineString from '$lib/components/geom/LineString.svelte';
	import VectorLayer from '$lib/components/layer/VectorLayer.svelte';
	import Map from '$lib/components/Map.svelte';
	import { onMount } from 'svelte';
	import { transform } from 'ol/proj';
	import Point from '$lib/components/geom/Point.svelte';

	interface Route {
		unit: string;
		runnername: string;
		lats: string;
		lngs: string;
		times: string;
		starttime: string;
	}

	interface LoggatorResponse {
		status: string;
		map: {
			imagelink: string;
			width: string;
			height: string;
			calstring: string;
		};
		routes: Route[];
	}

	let routes: [number, number][][] = [];

	onMount(async () => {
		const routesResponse = await fetch('routes.json');
		const routesJson = await routesResponse.json();
		routes = (routesJson as LoggatorResponse).routes.map(processRoutes);
	});

	function processRoutes(route: Route): [number, number][] {
		const lats = route.lats.split(';');
		const lons = route.lngs.split(';');

		return lats.map((lat, index) => {
			const coordinates = transform(
				[parseFloat(lons[index]), parseFloat(lat)],
				'EPSG:4326',
				'EPSG:3857'
			);
			return [coordinates[0], coordinates[1]];
		});
	}
</script>

<div class="wrapper">
	<nav class="container-fluid">
		<ul>
			<li><strong>Brand</strong></li>
		</ul>
		<ul>
			<li><a href="#">Link</a></li>
			<li><a href="#">Link</a></li>
			<li><a href="#" role="button">Button</a></li>
		</ul>
	</nav>

	<Map>
		<VectorLayer>
			{#each [...routes] as route}
				<LineString coords={route} />
			{/each}

			<Point coords={transform([5.702586, 46.018994], 'EPSG:4326', 'EPSG:3857')} />
		</VectorLayer>
	</Map>

	<div><input type="range" /></div>
</div>

<style>
	.wrapper {
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	:global(.ol-zoom-in),
	:global(.ol-zoom-out),
	:global(.ol-rotate-reset),
	:global(button:has(.ol-attribution-collapse)) {
		width: fit-content;
	}
</style>
