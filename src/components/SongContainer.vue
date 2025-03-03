<script setup>
// Libraries
import { ref } from 'vue';

// Components
import LabelTray from './LabelTray.vue';
import SongTitle from './SongTitle.vue';
import SongTimeline from './SongTimeline.vue';



defineProps({
    Title: String,
    Labels: Array
}) // end defineProps
</script>

<template>
    <div class="container">
        <SongTitle :Title="Title" />
        <SongTimeline :Labels="Labels" />
        <LabelTray ref="labelTray" />
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

    height: 100%;

    padding: 1%;
}
</style>

<script>

export default {
    methods: {
        test() {
            alert('test')
        }
    }
} // end export
</script>