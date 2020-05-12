<template>
    <div id="app">
        <nav-Component/>
        <main-map :passdata="mapData"/>
        <search-and-insert @returnMapData="getMapData" />
    </div>
</template>

<script>
import MainMap from './components/MainMap'
import SearchAndInsert from './components/searchAndInsert'
import navComponent from './components/nav'

export default {
    name: 'App',
    components: {
        navComponent,
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
        },
    }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Noto+Sans+TC:100,300,400,500,700,900&display=swap");
* {
    padding: 0;
    margin: 0;
}
#nav {
    position: fixed;
    z-index: 9999;
    top: 0;
}
#searchAndInsert {
    overflow: hidden;
    position: absolute;
    float: left;
    left: 5%;
    width: 0;
    height: inherit;
    transition: width .5s
}
#app {
    font-family: 'Noto Sans TC', 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
}
#map {
    position: relative;
    float: right;
    /* margin-top: 40px; */
    width: 100%;
    height: 60%;
    transition: width .5s
}
</style>
