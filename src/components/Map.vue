<template>
    <div
        style="height: 100%; width: 100%"
        class="map__container"
    >
        <button
            class="map__convert"
            @click="convertMap"
        >
            <img
                src="../assets/map.png"
                alt="map button"
            />
        </button>
        <l-map
            v-model="zoom"
            v-model:zoom="zoom"
            :center="locations[0]"
            @move="log('move')"
        >
            <l-tile-layer
                v-if="satellite"
                url="https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}"
            ></l-tile-layer>
            <l-tile-layer
                v-else
                url="https://tile.openstreetmap.org/{z}/{x}/{y}.png"
            ></l-tile-layer>

            <LControlLayers />

            <l-marker
                v-for="location of locations"
                :lat-lng="location"
                :key="location"
            >
                <l-icon
                    :icon-url="iconUrl"
                    :icon-size="iconSize"
                />
                <LPopup
                    :options="{
                        closeButton: false,
                        keepInView: true,
                    }"
                >
                    <div class="map__pop">
                        <img
                            src="../assets/download.jpg"
                            class="main__img"
                        />
                        <p class="pop__title">
                            هيئة قضايا الدولة, فرع التجمع الخامس
                        </p>
                        <p class="pop__info">
                            <img
                                src="../assets/gray-location.png"
                                width="16"
                            />
                            <span>شارع جامعة الدول العربية المهندسين42</span>
                        </p>
                        <p class="pop__info">
                            <img
                                src="../assets/phone.png"
                                width="16"
                            />
                            <span style="direction: ltr">02 - 37617046</span>
                        </p>
                        <button @click="fetchPlacesForCity()">
                            اعرف الاتجاهات <img src="../assets/arrow.svg" />
                        </button>
                    </div>
                </LPopup>
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
    props: ['locations'],
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
            satellite: false,
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
        convertMap() {
            this.satellite = !this.satellite;
        },
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
