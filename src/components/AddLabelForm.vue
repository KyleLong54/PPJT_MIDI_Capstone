<script setup>
// =================== Libraries ===================
import { ref } from 'vue';

// =================== Components ===================
import AddTitle from './AddTitle.vue';
import LabelCreationError from './LabelCreationError.vue';

// =================== Models ===================
import { LabelModel } from '@/models/LabelModel';

// Define properties
defineProps({
    Measures: Array
}); // end defineProps

// Get references to the inputs
const labelTitle = ref(null);
const measureSelect = ref(null);
// Get reference to the error message
const labelCreationErrorMessage = ref(null);

const emit = defineEmits(['addLabel'])

const addLabel = () => {
    // Debug message
    console.log('The form wants to add a label. Title: ' + labelTitle.value + ', Measure: ' + measureSelect.value);

    let labelToAdd;

    try {
        if (labelTitle.value && measureSelect.value) {
            // Create label and tell application to add it
            labelToAdd = new LabelModel(labelTitle.value, measureSelect.value);
            
            // Check to see whether label title is more than 25 characters
            if (labelToAdd.title.length < 25) {
                console.log('adding label');
                // If it is less, then add label
                emit('addLabel', labelToAdd);
                
                // If label was added with no errors, make sure that error message is not showing
                labelCreationErrorMessage.value.hideMessage();
            } else {
                console.log('Title too long');
                // If more, just show error message
                labelCreationErrorMessage.value.showMessage();
            } // end if
        } else {
            console.log('empty field(s)');
            // If either field is left empty, show error message
            labelCreationErrorMessage.value.showMessage();
        }
        // end if
    } catch (error) {
        // Toggle error message if error above
        // (just in case -> should be no error)
        labelCreationErrorMessage.value.showMessage();

        console.log('error in adding label');
    }
} // end addLabel
</script>

<template>
    <div class="container">
        <AddTitle></AddTitle>
        <div class="form">
            <div class="addTitle">
                <p>Title:</p>
                <input v-model="labelTitle" type="text" name="" id="">
            </div>
            <div class="selectMeasure">
                <p>Measure:</p>
                <select v-model="measureSelect" name="" id="">
                    <option v-for="(measure, index) in Measures" :value="index + 1">{{ index + 1 }}</option>
                </select>
            </div>
            <div class="buttonContainer">
                <button v-on:click="addLabel">Create Label</button>
                <LabelCreationError ref="labelCreationErrorMessage"></LabelCreationError>
            </div>
        </div>

    </div>
</template>

<style scoped>
.container {
    width: 50%;
}

.form {
    display: flex;
    flex-direction: column;
}

.addTitle {
    display: flex;
    align-items: center;

    gap: 50px;
}

.addTitle input {
    max-height: 30px;
}

.selectMeasure {
    display: flex;
    align-items: center;

    gap: 50px;
}

.buttonContainer {
    width: 100%;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

button {
    padding: 0px 0px 0px 0px;
    width: 95%;
}

select {
    min-width: 100px;
}
</style>