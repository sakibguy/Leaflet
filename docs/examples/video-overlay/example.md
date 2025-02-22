---
layout: tutorial_frame
title: Video Overlay Tutorial
---
<script>
	var map = L.map('map');

	var tiles = L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw', {
		maxZoom: 18,
		attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, ' +
			'Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
		id: 'mapbox/satellite-v9',
		tileSize: 512,
		zoomOffset: -1
	}).addTo(map);

	var videoUrls = [
		'https://www.mapbox.com/bites/00188/patricia_nasa.webm',
		'https://www.mapbox.com/bites/00188/patricia_nasa.mp4'
	],
	bounds = L.latLngBounds([[ 32, -130], [ 13, -100]]);

	map.fitBounds(bounds);

	var overlay = L.videoOverlay(videoUrls, bounds, {
		opacity: 0.8,
		interactive: false,
		autoplay: false
	});
	map.addLayer(overlay);

	var MyPauseControl,
		MyPlayControl,
 		pauseControl,
 		playControl;
        
	overlay.on('load', function () {
		MyPauseControl = L.Control.extend({
			onAdd: function() {
				var button = L.DomUtil.create('button');
				button.innerHTML = '⏸';
				L.DomEvent.on(button, 'click', function () {
					overlay.getElement().pause();
				});
				return button;
			}
		});
		MyPlayControl = L.Control.extend({
			onAdd: function() {
				var button = L.DomUtil.create('button');
				button.innerHTML = '▶️';
				L.DomEvent.on(button, 'click', function () {
					overlay.getElement().play();
				});
				return button;
			}
		});

		pauseControl = (new MyPauseControl()).addTo(map);
		playControl = (new MyPlayControl()).addTo(map);
	});

</script>
