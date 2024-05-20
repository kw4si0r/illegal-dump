<template>
    <div style="height: 75vh; width: 50vw;">
        <l-map :zoom="zoom" class="rounded map" :center="center" @update:zoom="zoomUpdated" @update:center="centerUpdated">
            <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" :attribution="OpenStreetMap" />

            <l-marker v-for="marker in markers" :key="marker" :lat-lng="[marker.lat, marker.lng]">
                <l-popup>
                    {{marker.label}}
                    <br/>
                    <hr/>
                    {{ [marker.lat, marker.lng] }}
                </l-popup>
            </l-marker>

        </l-map>
    </div>
</template>
<script>
import {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
} from "@vue-leaflet/vue-leaflet";
import "leaflet/dist/leaflet.css";

export default {
    name: 'LeafletMap',
    components: {
        LMap,
        LTileLayer,
        LMarker,
        LPopup
    },
    props: {
        markers: Array,
        zoom: {
            type: Number,
            default: 9
        },
        center : Array
    },
    data() {
        return {
            
        
        };
    },
    methods: {
        zoomUpdated (value) {
            this.$parent.zoomUpdated(value);
        },
        centerUpdated(value){
            let latlng = [value.lat, value.lng]
            this.$parent.centerUpdated(latlng);
        }
    }
};
</script>


<style scoped>
.map {
    border: 1px solid #ccc;
}
</style>
  