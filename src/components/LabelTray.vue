<script setup>
import Label from './Label.vue';
</script>

<template>
    <!--This will contain all of the labels that exist-->
    <div class="tray">
        <Label Title="Test Label" MeasureNum="1"></Label>
    </div>
</template>

<style scoped>
.tray {
    width: 100%;
    border: 1px solid purple;

    display: flex;
    justify-content: left;
}
</style>

<script>
export default {

} // end export
</script>