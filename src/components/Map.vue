<template>
    <div style="height: 100%; width: 100%">
        <l-map
            v-model="zoom"
            v-model:zoom="zoom"
            :center="[this.location.lat || 0, this.location.lng || 0]"
            @move="log('move')"
        >
            <l-tile-layer
                url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
            ></l-tile-layer>
            <LControlLayers />

            <l-marker
                :lat-lng="[
                    [45.51, -122.68],
                    [37.77, -122.43],
                    [34.04, -118.2],
                ]"
            >
                <l-icon :icon-url="iconUrl" :icon-size="iconSize" />
            </l-marker>
        </l-map>
    </div>
</template>
<script>
import {
    LMap,
    LIcon,
    LTileLayer,
    LMarker,
    LControlLayers,
    LTooltip,
    LPopup,
    LPolyline,
    LPolygon,
    LRectangle,
} from '@vue-leaflet/vue-leaflet';
import 'leaflet/dist/leaflet.css';
export default {
    name: 'Map',
    props: ['lat', 'lng'],
    components: {
        LMap,
        LIcon,
        LTileLayer,
        LMarker,
        LControlLayers,
        LTooltip,
        LPopup,
        LPolyline,
        LPolygon,
        LRectangle,
    },
    data() {
        return {
            zoom: 5,
            iconWidth: 40,
            iconHeight: 50,
        };
    },
    computed: {
        location() {
            return { lat: this.lat, lng: this.lng };
        },
        iconUrl() {
            return 'pin.png';
        },
        iconSize() {
            return [this.iconWidth];
        },
    },
    mounted() {},
    methods: {
        log(a) {
            console.log(a);
        },
        changeIcon() {
            this.iconWidth += 2;
            if (this.iconWidth > this.iconHeight) {
                this.iconWidth = Math.floor(this.iconHeight / 2);
            }
        },
    },
};
</script>

<style lang="scss">
@import '../styles/map.scss';
</style>
