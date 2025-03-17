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

const emit = defineEmits(['skip']);

const skip = (measureNum) => {
    emit('skip', measureNum);
} // end skip

const addLabel = (labelToAdd) => {
    labelTray.value.addLabel(labelToAdd);
}

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

    gap: 5px;

    padding: 1%;
}
</style>