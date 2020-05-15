<template>
    <div id="app">
        <nav-Component :collapse="collapseFn" />
        <search-and-insert @returnMapData="getMapData" :collapse="collapseFn" />
        <main-map :passdata="mapData" />
    </div>
</template>

<script>
import MainMap from './components/MainMap'
import SearchAndInsert from './components/searchAndInsert'
import navComponent from './components/nav'

import jQuery from 'jquery';
import "bootstrap";
import "bootstrap/dist/css/bootstrap.css";

export default {
    name: 'App',
    components: {
        navComponent,
        MainMap,
        SearchAndInsert
    },
    data () {
        return {
            mapData: {},
            collapseFn: null,
        }
    },
    computed: {
        getWindowWidth() {

        }
    },
    created() {
        this.collapseFn = this.collapse;
    },
    methods: {
        getMapData() {
            this.mapData = arguments[0];
        },
        collapse() {
            jQuery('#searchAndInsert').toggleClass('open');
            jQuery('#map').toggleClass('open');
        }
    }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Noto+Sans+TC:100,300,400,500,700,900&display=swap");

@media screen and (min-width: 1025px) {
    .searchAndInsert {
        float: left;
        left: 5%;
        width: 0;
        transition: width .5s;
    }
    .searchAndInsert.open {
        width: 30%;
    }
    .MainMap {
        float: right;
        width: 100%;
        height: 60%;
        transition: width .5s;
    }
    .MainMap.open {
        width: 60%;
    }
}
@media screen and (max-width: 1024px) and (min-width: 960px){
    .searchAndInsert {
        float: top;
        top: 2%;
        left: 20%;
        width: 60%;
        height: 0;
        transition: height .5s;
    }
    .searchAndInsert.open {
        height: 70%;
    }
    .MainMap {
        float: bottom;
        width: 100%;
        transition: margin-top .5s;
    }
    .MainMap.open {
        margin-top: 50%;
    }
}
@media screen and (max-width: 960px) {
     .searchAndInsert {
        float: top;
        top: 1%;
        left: 20%;
        width: 60%;
        height: 0;
        transition: height .5s;
    }
    .searchAndInsert.open {
        height: 70%;
    }
    .MainMap {
        float: bottom;
        width: 100%;
        transition: margin-top .5s;
    }
    .MainMap.open {
        margin-top: calc(75% - 8vw);
    }
}
@media screen and (max-width: 760px) {
     .searchAndInsert {
        float: top;
        top: 1%;
        left: 20%;
        width: 60%;
        height: 0;
        transition: height .5s;
    }
    .searchAndInsert.open {
        height: 90%;
    }
    .MainMap {
        float: bottom;
        width: 100%;
        transition: margin-top .5s;
    }
    .MainMap.open {
        margin-top: 80%;
    }
}
@media screen and (max-width: 640px) {
     .searchAndInsert {
        float: top;
        top: 0;
        left: 0;
        width: 100%;
        height: 0;
        transition: height .5s;
    }
    .searchAndInsert.open {
        height: 70%;
    }
    .MainMap {
        float: bottom;
        width: 100%;
        transition: margin-top .5s;
    }
    .MainMap.open {
        margin-top: 100% ;
    }
}
@media screen and (max-width: 500px) {
    .searchAndInsert.open {
        height: 70%;
    }
    .MainMap.open {
        margin-top: 120% ;
    }
}
@media screen and (max-width: 400px) {
    .searchAndInsert.open {
        height: 80%;
    }
    .MainMap.open {
        margin-top: 135% ;
    }
}
@media screen and (max-width: 320px) {
    .searchAndInsert.open {
        height: 90%;
    }
    .MainMap.open {
        margin-top: 150% ;
    }
}
* {
    padding: 0;
    margin: 0;
}
#app {
    font-family: 'Noto Sans TC', 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    width: 100vw;
    height: 100vh;
    color: #2c3e50;
}

#nav {
    position: fixed;
    z-index: 9999;
    top: 0;
}
.searchAndInsert {
    overflow: hidden;
    position: absolute;
}
.MainMap {
    position: relative;
}
</style>
