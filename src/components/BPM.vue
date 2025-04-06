<template>
    <!-- BPM changing -->
    <div class="bpmContainer">
        <p>BPM</p>
        <!-- User will enter new desired BPM percentage here -->
        <input type="text" value="100%">
    </div>
</template>

<style scoped>
.bpmContainer {
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    gap: 2px;

    max-width: 48%;
    height: 100%;

    padding: 1%;
}

input {
    max-width: 80%;
}

/* Colors */
.bpmContainer {
    background-color: var(--color-bg);

    border: 2px solid var(--color-border);
    border-radius: 10px;
}

input {
    text-align: center;
}
</style>