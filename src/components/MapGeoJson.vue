<template>
  <div>
    <div style="height: 10%; overflow: auto;">
      <h3>GeoJSON</h3>
      <span v-if="loading">Loading...</span>
      <label for="checkbox">GeoJSON Visibility</label>
      <input id="checkbox" v-model="show" type="checkbox" />
      <label for="checkboxTooltip">Enable tooltip</label>
      <input id="checkboxTooltip" v-model="enableTooltip" type="checkbox" />
      <input v-model="fillColor" type="color" />
      <br />
    </div>
	<div class="appMap">
		<l-map :zoom="zoom" :center="center" style="height: 90%">
		  <l-tile-layer :url="url" :attribution="attribution" />
		  <l-geo-json
			v-if="show"
			:geojson="geojson"
			:options="options"
			:options-style="styleFunction"
		  />
		  <l-marker :lat-lng="marker" />
		</l-map>
	</div>
  </div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LGeoJson } from "vue2-leaflet";

import axios from "axios";

export default {
  name: "MapGeoJson",
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
    LMarker
  },
  data() {
    return {
      loading: false,
      show: true,
      enableTooltip: true,
      zoom: 8,
      center: [-23.6234345, -45.4134647],
      geojson: null,
      fillColor: "#e4ce7f",
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: latLng(-23.6234345, -45.4134647)
    };
  },
  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction
      };
    },
    styleFunction() {
      const fillColor = this.fillColor; // important! need touch fillColor in computed for re-calculate when change fillColor
      return () => {
        return {
          weight: 2,
          color: "#ECEFF1",
          opacity: 1,
          fillColor: fillColor,
          fillOpacity: 1
        };
      };
    },
    onEachFeatureFunction() {
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.bindTooltip(
          "<div>code:" +
            feature.properties.code +
            "</div><div>nom: " +
            feature.properties.nom +
            "</div>",
          { permanent: false, sticky: true }
        );
      };
    }
  },
  created() {
    this.loading = true;
    axios
      .get(
        "http://localhost:3000/lines.json"
      )
      .then(response => {
        this.geojson = response.data;
        this.loading = false;
		 console.log(response.data);
      });
  }
};
</script>

<style scoped>
    .appMap {
        width: 70%;
        height: 400px;
    }
</style>
