<template>
    <div id="app">
        <main-map :passdata="mapData"/>
        <search-and-insert @returnMapData="getMapData"/>
    </div>
</template>

<script>
import MainMap from './components/MainMap'
import SearchAndInsert from './components/searchAndInsert'


export default {
    name: 'App',
    components: {
        MainMap,
        SearchAndInsert
    },
    data () {
        return {
            mapData: [],
            test: null,
        }
    },
    methods: {
        getMapData() {
            this.mapData = arguments[0];
        }
    }
}
</script>

<style>
* {
    padding: 0;
    margin: 0;
}
#searchAndInsert {
    position: absolute;
    left: 0;
    top: 0;
    z-index: 1;
}
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    /* text-align: center; */
    color: #2c3e50;
    /* margin-top: 60px; */
}
#map {
    z-index: -999;
}
</style>
