<template>
    <div id="visualization">
        <l-map ref="map"
               :zoom="zoom"
               :center="center"
               style="height: 800px;">
            <l-tile-layer :url="url"
                          :attribution="attribution"></l-tile-layer>
            <l-geo-json :geojson="statesData.geojson"
                        :options="statesData.options"></l-geo-json>
            <l-marker :lat-lng="marker"></l-marker>
        </l-map>
    </div>
</template>

<script>
    import L from 'leaflet';
    import { LMap, LTileLayer, LMarker, LGeoJson } from 'vue2-leaflet';
    import data from '../assets/geojson/us-states';

    delete L.Icon.Default.prototype._getIconUrl;
    L.Icon.Default.mergeOptions({
        iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
        iconUrl: require('leaflet/dist/images/marker-icon.png'),
        shadowUrl: require('leaflet/dist/images/marker-shadow.png')
    });
    export default {
        name: 'Visualization',
        props: {
            msg: String
        },
        components: {
            LMap,
            LTileLayer,
            LMarker,
            LGeoJson
        },
        data () {
            return {
                zoom: 5,
                center: L.latLng(36, 120),
                url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
                attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
                marker: L.latLng(36, -120),
                statesData: {
                    geojson: data.statesData,
                    options: {

                    }
                }
            }
        }
    }
</script>

<style scoped lang="scss">
    @import "~leaflet/dist/leaflet.css";
</style>