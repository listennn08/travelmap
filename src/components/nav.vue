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

    methods: {
        collapse() {
            // console.log(jQuery('#searchAndInsert').css('width') == "0px");

            if (jQuery(window).width() < 960) {
                if (jQuery('#searchAndInsert').css('height') == "0px") {
                    jQuery('#searchAndInsert').css('height', "30%");
                    jQuery("#map").css('height', "30%");
                } else {
                    jQuery('#searchAndInsert').css('height', "0");
                    jQuery("#map").css('height', "60%");
                }
            } else {
                console.log(jQuery('#searchAndInsert').height());
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
<style lang="sass" scope>
    button
        outline: none
</style>
