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
} // end addLabel

defineExpose({
    addLabel
});
</script>

<template>
    <!--This will contain all of the labels that exist-->
    <div class="tray">
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
    border: 1px solid purple;

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
</style>