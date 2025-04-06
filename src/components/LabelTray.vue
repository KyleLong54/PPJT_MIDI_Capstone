<script setup>
// =================== Libraries ===================
import { ref } from 'vue';

// =================== Components ===================
import Label from './Label.vue';

// =================== Models ===================
import { LabelModel } from '@/models/LabelModel';

// Define properties
defineProps({
    Labels: Array,
}) // end defineProps

// Create reference to array for all labels
const labels = ref([]);
// Create reference to label tray (for hiding it)
const tray = ref(null);

// Sorting label array to display them descending
labels.value.sort((a, b) => a.measureNum - b.measureNum);

// Define Emits
const emit = defineEmits(['skip']);

// Tells application to skip to a given measure
const skip = (measureNum) => {
    emit('skip', measureNum);
} // end skip

// Adds a label
const addLabel = (labelToAdd) => {
    labels.value.push(labelToAdd);

    // Sorting label array to display them descending
    labels.value.sort((a, b) => a.measureNum - b.measureNum);

    // Make sure that label tray is showing
    tray.value.style.border = '2px solid var(--color-border)';
} // end addLabel

defineExpose({
    addLabel
});
</script>

<template>
    <!--This will contain all of the labels that exist-->
    <div ref="tray" class="tray">
        <div class="label" v-for="label in labels">
            <!--Ensures that all labels are lined up with their measure-->
            <div class="preSpacer" v-for="n in label.measureNum - 1"></div>
            <!--Moves the label slightly to line it up more-->
            <div class="smallSpacer"></div>
            <!--Label-->
            <Label :Title="label.title" :MeasureNum="label.measureNum" @skip="skip"></Label>
            <!--Ensures that the next label is not on the same line-->
            <div class="postSpacer"></div>
        </div>
    </div>
</template>

<style scoped>
.tray {
    width: 100%;

    display: flex;
    flex-direction: column;
    gap: 5px;
    justify-content: left;
}

.label {
    display: flex;
    justify-content: left;
}

.preSpacer {
    min-width: 36.6px;
}

.smallSpacer {
    min-width: 4px;
}

.postSpacer {
    width: 100%;
}

/* Colors */
.tray {
    background-color: var(--color-bg-panel);

    border: 0px solid var(--color-border);
    border-radius: 8px;
}
</style>