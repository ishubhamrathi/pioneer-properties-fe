<script lang="ts">
	import { browser } from '$app/environment';

	let mapContainer: HTMLElement;
	let map: any;

	if (browser) {
		import('leaflet').then(async (L) => {
			const Leaflet = L.default;
			await import('leaflet/dist/leaflet.css');

			if (mapContainer) {
				map = Leaflet.map(mapContainer, { 
					zoomControl: true,
					zoomSnap: 0.5,
					zoomDelta: 1
				}).setView([51.505, -0.09], 13);

				// Request user location on map load
				map.locate({ setView: true, maxZoom: 16 });

				// Position zoom control to bottom right
				map.zoomControl.setPosition('bottomright');

				// Add locate control
				const locateControl = Leaflet.control({ position: 'bottomright' });
				locateControl.onAdd = function() {
					const button = Leaflet.DomUtil.create('button', 'locate-button');
					button.innerHTML = '📍';
					button.title = 'Go to current location';
					button.onclick = function() {
						map.locate({ setView: true, maxZoom: 16 });
					};
					return button;
				};
				locateControl.addTo(map);

				// Define base layers
				const osm = Leaflet.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
					attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
					updateWhenZooming: false,
					updateWhenIdle: true,
					keepBuffer: 2,
					loading: () => console.log('tiles loading')
				}).addTo(map);

				const satellite = Leaflet.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
					attribution: 'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community',
					updateWhenZooming: false,
					updateWhenIdle: true,
					keepBuffer: 2
				});

				const dark = Leaflet.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
					attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions">CARTO</a>',
					subdomains: 'abcd',
					updateWhenZooming: false,
					updateWhenIdle: true,
					keepBuffer: 2
				}).addTo(map);

				// Add layer control
				Leaflet.control.layers({
					'OpenStreetMap': osm,
					'Satellite': satellite,
					'Aerial': aerial,
					'Dark Mode': dark
				}).addTo(map);

				map.invalidateSize();
			}
		});
	}
</script>

<div bind:this={mapContainer} style="width: 100%; height: 100vh;"></div>

<style>
	.leaflet-control-container {
		font-family: inherit;
	}

	/* Style the layer control as overlay */
	.leaflet-control-layers {
		position: absolute;
		bottom: 20px;
		left: 50%;
		transform: translateX(-50%);
		background: rgba(255, 255, 255, 0.8);
		backdrop-filter: blur(10px);
		border-radius: 50px;
		padding: 10px;
		box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
	}

	.leaflet-control-layers-list {
		display: flex;
		gap: 10px;
	}

	.leaflet-control-layers-selector {
		display: none;
	}

	.leaflet-control-layers label {
		background: rgba(255, 255, 255, 0.5);
		border-radius: 20px;
		padding: 8px 16px;
		cursor: pointer;
		transition: background 0.3s;
	}

	.leaflet-control-layers label:hover {
		background: rgba(255, 255, 255, 0.8);
	}

	/* Style the locate button */
	.locate-button {
		background: rgba(255, 255, 255, 0.8);
		border: none;
		border-radius: 50%;
		width: 40px;
		height: 40px;
		cursor: pointer;
		font-size: 18px;
		margin-left: 10px;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
		transition: background 0.3s;
	}

	.locate-button:hover {
		background: rgba(255, 255, 255, 1);
	}
</style>
