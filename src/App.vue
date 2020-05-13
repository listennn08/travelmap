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
    created() {
        this.collapseFn = this.collapse;
    },
    methods: {
        getMapData() {
            this.mapData = arguments[0];
        },
        collapse() {
            // console.log(jQuery('#searchAndInsert').css('width') == "0px");
            if (jQuery(window).width() <= 1024 && jQuery(window).width() > 560) {
                if (jQuery('#searchAndInsert').width() == 0) jQuery('#searchAndInsert').width("50%");
                if (jQuery('#searchAndInsert').height() == 0) {
                    jQuery('#searchAndInsert').css('height', "80%");
                    jQuery('#map').css('margin-top', '70%')
                } else {
                    jQuery('#searchAndInsert').css('height', "0");
                    jQuery("#map").css('margin-top', "0");
                }
            } else if (jQuery(window).width() <= 560) {
                console.log('here')
                if (jQuery('#searchAndInsert').width() == 0) jQuery('#searchAndInsert').width("100%");
                if (jQuery('#searchAndInsert').height() == 0) {
                    jQuery('#searchAndInsert').css('height', "90%");
                    jQuery('#map').css('margin-top', '150%')
                } else {
                    jQuery('#searchAndInsert').css('height', "0");
                    jQuery("#map").css('margin-top', "0");
                }
            } else {
                if (jQuery('#searchAndInsert').css('width') == "0px") {
                    jQuery('#searchAndInsert').css('width', "30%");
                    jQuery("#map").css('width', "60%");
                } else {
                    jQuery('#searchAndInsert').css('width', "0");
                    jQuery("#map").css('width', "100%");
                }
            }
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
    .MainMap {
        float: right;
        width: 100%;
        height: 60%;
        transition: width .5s;
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
    .MainMap {
        float: bottom;
        width: 100%;
        transition: margin-top .5s;
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
    .MainMap {
        float: bottom;
        width: 100%;
        transition: margin-top .5s;
    }
}
@media screen and (max-width: 580px) {
     .searchAndInsert {
        float: top;
        top: 0;
        left: 0;
        width: 100%;
        height: 0;
        transition: height .5s;
    }
    .MainMap {
        float: bottom;
        width: 100%;
        transition: margin-top .5s;
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
