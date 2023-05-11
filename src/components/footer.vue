<script>
    export default {
        props: ['year'],
    }
</script>

<template>
    <footer>
        <p>MapMn.app <span id="current-year"><!--©-->{{ this.year }}</span></p>
    </footer>
</template>

<style>
footer {
    background-color: var(--main_color_1);
    height: 50px;
    text-align: center;
    padding: 10px;
    color: white;
}
</style>