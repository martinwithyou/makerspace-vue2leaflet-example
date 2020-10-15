<template>
    <div id="visualization">
        <l-map ref="map"
               :zoom="zoom"
               :center="center"
               style="height: 800px;">
            <l-tile-layer
                v-for="tileProvider in tileProviders"
                :key="tileProvider.name"
                :name="tileProvider.name"
                :visible="tileProvider.visible"
                :url="tileProvider.url"
                :attribution="tileProvider.attribution"
                layer-type="base"/>
            <l-geo-json :geojson="statesData.geojson"
                        :options="statesData.options"></l-geo-json>
            <l-marker :lat-lng="marker"></l-marker>
            <!-- <l-tooltip :latLng="marker">Hello!</l-tooltip>
            <l-popup :latLng="marker">Hello!</l-popup> -->
            <l-circle
                :lat-lng="circle.center"
                :radius="circle.radius"
                :color="circle.color"
                :opacity="circle.opacity"
                :fillColor="circle.fillColor"
                />
            <l-control-layers position="topright"  ></l-control-layers>
            <l-polygon :lat-lngs="polygon.latlngs" :color="polygon.color"></l-polygon>
        </l-map>
    </div>
</template>

<script>
    import L from 'leaflet';
    import { 
        LMap, 
        LTileLayer, 
        LMarker, 
        LGeoJson 
    } from 'vue2-leaflet';
    import { 
        LCircle, 
        LControlLayers,
        Licon, 
        LPolygon,
    } from 'vue2-leaflet';
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
            LGeoJson,
            LCircle,
            LControlLayers,
            Licon,
            LPolygon,
        },
        data () {
            return {
                polygon: {
                    latlngs: [
                        //[31.32, 120.62],
                        [31.39, 120.95], 
                        //[31.16, 120.63],
                        [31.86, 120.55],
                        [31.45, 121.1]
                    ],
                    color: 'green'
                },
                iconSize: 64,
                zoom: 11,
                center: L.latLng(31.299379, 120.619585),
                url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
                attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
                marker: L.latLng(31.299379, 120.619585),
                statesData: {
                    geojson: data.statesData,
                    options: {

                    }
                },
                circle: {
                    center: [31.299379, 120.619585],
                    radius: 5000,
                    color: 'blue',
                    opacity: 0.7,
                    fillColor:'green'
                },
                tileProviders: [
                    {
                    name: 'OpenStreetMap',
                    visible: true,
                    attribution:
                        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
                    url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
                    },
                    {
                    name: 'OpenTopoMap',
                    visible: false,
                    url: 'https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png',
                    attribution:
                        'Map data: &copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)',
                    },
                ],
            }
        },
        computed: {
            dynamicSize () {
                return [this.iconSize, this.iconSize * 1.15];
            },
            dynamicAnchor () {
                return [this.iconSize / 2, this.iconSize * 1.15];
            }
        }
    }
</script>

<style scoped lang="scss">
    @import "~leaflet/dist/leaflet.css";
</style>