<template lang="pug">
    nav#nav.nav
        li
            button.btn.btn-default(@click="collapse()") ☰
</template>
<script>
import jQuery from 'jquery';
import "bootstrap";
import "bootstrap/dist/css/bootstrap.css";

export default {

    methods: {
        collapse() {
            // console.log(jQuery('#searchAndInsert').css('width') == "0px");
            if (jQuery('#searchAndInsert').css('width') == "0px") {
                jQuery('#searchAndInsert').css('width', "30%");
                jQuery("#map").css('width', "60%");
            } else {
                jQuery('#searchAndInsert').css('width', "0");
                jQuery("#container").css('margin-left', "0");
                jQuery("#map").css('width', "100%");
            }
        }
    }
}
</script>
