<template>
    <div id="app">
        <nav-Component />
        <main-map :passdata="mapData"/>
        <search-and-insert @returnMapData="getMapData"/>
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
        }
    }
}
</script>

<style>
* {
    padding: 0;
    margin: 0;
}
#nav {
    position: fixed;
    top: 0;
}
#searchAndInsert {
    position: absolute;
    float: left;
    left: 2%;
    /* top: 0;
    bottom: 0;
    right: 0; */
    width: 30%

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
    position: relative;
    float: right;
    /* margin-top: 50px; */
    width: 60%;
    height: 70%;
}
</style>
