<script setup>
// =================== Libraries ===================
import { ref } from 'vue';

// =================== Components ===================
import LabelTray from './LabelTray.vue';
import SongTitle from './SongTitle.vue';
import SongTimeline from './SongTimeline.vue';

// Get reference to the Label Tray
const labelTray = ref(null);

// Define properties
defineProps({
    Title: String,
    Labels: Array,
    Mesaures: Array
}) // end defineProps

// Setup Emits
const emit = defineEmits(['skip']);

// Tells the application to skip to a given measure
const skip = (measureNum) => {
    emit('skip', measureNum);
} // end skip

// Adds a label to the label tray
const addLabel = (labelToAdd) => {
    labelTray.value.addLabel(labelToAdd);
} // end addLabel

// Allow the method to be accessed by parents
defineExpose({
    addLabel
});
</script>

<template>
    <div class="container">
        <SongTitle :Title="Title" />
        <SongTimeline :-measures="Mesaures" @skip="skip" />
        <LabelTray ref="labelTray" @skip="skip" />
    </div>
</template>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;

    gap: 2px;

    padding: 1%;
}
</style>