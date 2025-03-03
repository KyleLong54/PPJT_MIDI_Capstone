<script setup>
import Label from './Label.vue';
import { LabelModel } from '@/models/LabelModel';

defineProps({
    Labels: Array,
}) // end defineProps


const labels = [];

// Create test labels
const label1 = new LabelModel("Test1", 1);
labels.push(label1);
const label2 = new LabelModel("This is a label", 3);
labels.push(label2);
labels.push(new LabelModel("This is the 2nd label", 2))

// Sorting label array to display them descending
labels.sort((a, b) => a.measureNum - b.measureNum);
</script>

<template>
    <!--This will contain all of the labels that exist-->
    <div class="tray">
        <div class="label" v-for="label in labels">
            <div class="preSpacer" v-for="n in label.measureNum - 1"></div>
            <Label :Title="label.title" :MeasureNum="label.measureNum"></Label>
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
    min-width: 40px;
}

.postSpacer {
    width: 100%;
}
</style>

<script>
export default {
    methods: {
        test() {
            alert("Test");
        }
    }
} // end export
</script>