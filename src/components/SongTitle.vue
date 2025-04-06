<script setup>
// Define properties
defineProps({
    Title: String
}) // end defineProps
</script>

<template>
    <!-- Displays the title of the uploaded song -->
    <div class="aligner">
        <p>{{ Title }}</p>
    </div>
</template>

<style scoped>
p {
    max-width: 80%;
    padding: 0px 6px 0px 6px;
}

.aligner {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: left;
    justify-content: left;

    width: 100%;
}

/* Colors */
p {
    border: 1px solid var(--color-border);
    border-radius: 5px;

    background-color: var(--color-bg-panel);
}
</style>

<script>
export default {

} // end export
</script>