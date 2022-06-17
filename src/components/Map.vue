<template>
    <div id="map">
    </div>
    <p>* please know that these locations are generated randomly based on lat,lng so most of them may be in the
        ocean!</p>
</template>
<script>

import mapboxgl from 'mapbox-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

export default {
    data() {
        return {
            map: '',
        }
    },
    props: ['lng', 'lat'],

    methods: {
        loadMap(lng, lat) {
            mapboxgl.accessToken = 'pk.eyJ1Ijoic2FyaW5hdmFpaSIsImEiOiJja3FhZnA5eWgwejdhMm5vd2o0cGttc2hqIn0.HgbfsembHAi8QtXAoJ5S7w';
            this.map = new mapboxgl.Map({
                container: 'map',
                style: 'mapbox://styles/mapbox/streets-v11',
                center: [lng, lat],
                zoom: 3
            });
        }
    },

    mounted: function () {
        this.loadMap(this.lng, this.lat)
    },

    updated: function () {
        this.map.flyTo({
            center: [
                this.lng,
                this.lat
            ],
            essential: true
        })
        this.map.resize();
    },
}
</script>
<style>
#map {
    width: 100%;
    height: 300px;
}

p {
    margin-top: 1rem;
    font-size: 0.75rem;
    line-height: 1.25;
    font-style: italic;
}
</style>