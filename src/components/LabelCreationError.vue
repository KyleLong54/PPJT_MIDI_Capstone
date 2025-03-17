<script setup>
// =================== Libraries ===================
import { ref } from 'vue';

// Reference to error message text
const message = ref(null);

// Toggles the visibility of the error message
const toggle = () => {
    if (message.value.style.visibility == 'hidden') {
        message.value.style.visibility = 'visible';
    } else {
        message.value.style.visibility = 'hidden';
    } // end if
} // end toggle

const showMessage = () => {
    message.value.style.visibility = 'visible';
}

const hideMessage = () => {
    message.value.style.visibility = 'hidden';
}

defineExpose({
    showMessage,
    hideMessage
});
</script>

<template>
    <p ref="message" class="error">The title you entered is too long, or fields are empty. Please fill both fields and limit titles to 25 characters or less.</p>
</template>

<style scoped>
.error {
    color: red;
    visibility: hidden;
}
</style>