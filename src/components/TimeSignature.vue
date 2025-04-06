<template>
    <!-- Time Signature changing -->
    <div class="timeSigContainer">
        <p>Time Signature</p>
        <!-- User will enter new desired time signature here -->
        <input type="text" value="4 | 4">
    </div>
</template>

<style scoped>
.timeSigContainer {
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
.timeSigContainer {
    background-color: var(--color-bg);

    border: 2px solid var(--color-border);
    border-radius: 10px;
}

input {
    text-align: center;
}
</style>