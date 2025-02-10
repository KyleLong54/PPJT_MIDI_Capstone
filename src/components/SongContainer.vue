<script setup>
import LabelTray from './LabelTray.vue';
import SongTitle from './SongTitle.vue';
import SongTimeline from './SongTimeline.vue';

defineProps({
    Title: String,
}) // end defineProps
</script>

<template>
    <div class="container">
        <SongTitle :Title="Title" />
        <SongTimeline />
        <LabelTray />
    </div>
</template>

<style scoped>
.container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;

    gap: 5px;

    width: 97%;
    height: 100%;

    padding: 1%;
}
</style>

<script>
export default {

} // end export
</script>