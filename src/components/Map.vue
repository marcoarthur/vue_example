<template>
	<div class="appMap">
	<h2> {{ title }} </h2>
		<l-map :zoom="zoom" :center="center">
			<l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
			<l-marker :lat-lng="marker"></l-marker>
		</l-map>
	</div>
</template>

<script>
import { LMap, LTileLayer, LMarker } from 'vue2-leaflet'

var uptit = function() {
	return Date.now + "Hi"
}

export default {
	name: 'Map',
	components: { LMap, LTileLayer, LMarker },
	data: function () { 
		return {
			zoom:13,
			center: L.latLng(47.413220, -1.219482),
			url:'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
			attribution:'&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
			marker: L.latLng(47.413220, -1.219482),
			title: uptit(),
		}
	}
}
</script>

<style scoped>
    .appMap {
        width: 70%;
        height: 400px;
    }
</style>
