<template lang="pug">
    #map
        l-map(
            ref="map"
            :zoom="zoom"
            :center="center"
            :options="options"
            style="height: 100vh;"
        )
            l-tile-layer(
                :url="url"
                attribution="attribution"
            )
            l-marker(
                ref="location"
                :lat-lng="center"
            )
                l-popup 你的位置
            l-marker(
                v-for="item in data"
                :lat-lng="item.local"
                :key="item.id"
            )
                l-icon(
                    :icon-url="item.count > 10000 ? icon.type.red : icon.type.green"
                    :shadow-url="icon.shdowUrl"
                    :icon-size="icon.iconSize"
                    :icon-anchor="icon.iconAnchor"
                    :popup-anchor="icon.popupAnchor"
                    :shadow-size="icon.shadowSize"
                )
                l-popup
                    h2 {{ item.name }}
                    p(v-show="item.count!=''") 預估觀光人數: {{ item.count }}

</template>
<script>
export default {
    data() {
        return {
            data: [],
            zoom: 13,
            center: [22.612961, 120.304167],
            url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
            attribution: `© <a href="http://osm.org/copyright">OpenStreetMap</a> contributors`,
            options: {
                zoomControl: false,
            },
            icon: {
                type: {
                    black:
                        "https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-black.png",
                    gold:
                        "https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-gold.png",
                    green:
                        "https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-green.png",
                    oringe:
                        "https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-orange.png",
                    red:
                        "https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png",
                },
                shadowUrl:
                    "https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png",
                iconSize: [25, 41],
                iconAnchor: [12, 41],
                popupAnchor: [1, -34],
                shadowSize: [41, 41]
            }
        }
    },
    props: {
        passdata: Array,
    },
    watch: {
        passdata: function () {
            this.data = [];
            this.passdata.forEach( (el) => {
                this.data.push({
                    id: el.id,
                    name: el.name,
                    local: el.local,
                    count: el.count,
                })
            })
        }
    },
    mounted() {
        // 等地圖創建後執行
        this.$nextTick(() => {
        // 獲得目前位置
            navigator.geolocation.getCurrentPosition(position => {
                const p = position.coords;
                // 將中心點設為目前的位置
                this.center = [p.latitude, p.longitude] ;
                // 將目前的位置的標記點彈跳視窗打開
                this.$refs.location.mapObject.openPopup();
            }, (error) => {
                this.center = [this.center[0]+0.005, this.center[1]];
                this.$refs.location.mapObject.openPopup();
            });
        });
    }
}
</script>
<style lang="sass" scoped>
html, body 
    padding: 0
    margin: 0
</style>
