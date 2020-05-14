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
}
</script>
<style lang="sass" scope>
    button
        outline: none
</style>
