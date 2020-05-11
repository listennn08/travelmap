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
            // jQuery('#searchAndInsert')
            if (document.getElementById("searchAndInsert").style.width == 0) {
                document.getElementById("searchAndInsert").style.width = "250px";
                document.getElementById("map").style.marginLeft = "250px";
            } else {
                document.getElementById("searchAndInsert").style.width = "0px";
                document.getElementById("map").style.marginLeft = "0";
            }

        }
    }
}
</script>
