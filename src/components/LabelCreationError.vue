<script setup>
// =================== Libraries ===================
import { ref } from 'vue';

// Reference to error message text
const message = ref(null);

// Shows the error message
const showMessage = () => {
    message.value.style.visibility = 'visible';
    // Make sure it has a height
    message.value.style.height = 'fit-content';
}

// Hides the error message
const hideMessage = () => {
    message.value.style.visibility = 'hidden';
    // Also make it have no height
    message.value.style.height = '0px';
}

defineExpose({
    showMessage,
    hideMessage
});
</script>

<template>
    <!-- Error message for label creation -->
    <p ref="message" class="error">The title you entered is too long, or fields are empty. Please fill both fields and limit titles to 25 characters or less.</p>
</template>

<style scoped>
.error {
    color: red;
    visibility: hidden;
    height: 0px;
}
</style>