<template lang="pug">
    nav#nav.nav
        li
            button.btn.btn-light.h4(@click="collapse()") ☰ 新增/查詢
</template>
<script>
import jQuery from 'jquery';
import "bootstrap";
import "bootstrap/dist/css/bootstrap.css";

export default {
    props: {
        collapse: Function
    },
    methods: {
        // collapse() {
        //     // console.log(jQuery('#searchAndInsert').css('width') == "0px");

        //     if (jQuery(window).width() <= 1024 && jQuery(window).width() > 560) {
        //         if (jQuery('#searchAndInsert').width() == 0) jQuery('#searchAndInsert').width("50%");
        //         if (jQuery('#searchAndInsert').height() == 0) {
        //             jQuery('#searchAndInsert').css('height', "80%");
        //             jQuery('#map').css('margin-top', '70%')
        //         } else {
        //             jQuery('#searchAndInsert').css('height', "0");
        //             jQuery("#map").css('margin-top', "0");
        //         }
        //     } else if (jQuery(window).width() <= 560) {
        //         console.log('here')
        //         if (jQuery('#searchAndInsert').width() == 0) jQuery('#searchAndInsert').width("100%");
        //         if (jQuery('#searchAndInsert').height() == 0) {
        //             jQuery('#searchAndInsert').css('height', "90%");
        //             jQuery('#map').css('margin-top', '150%')
        //         } else {
        //             jQuery('#searchAndInsert').css('height', "0");
        //             jQuery("#map").css('margin-top', "0");
        //         }
        //     } else {
        //         if (jQuery('#searchAndInsert').css('width') == "0px") {
        //             jQuery('#searchAndInsert').css('width', "30%");
        //             jQuery("#map").css('width', "60%");
        //         } else {
        //             jQuery('#searchAndInsert').css('width', "0");
        //             jQuery("#map").css('width', "100%");
        //         }
        //     }
        // }
    }
}
</script>
<style lang="sass" scope>
    button
        outline: none
</style>
