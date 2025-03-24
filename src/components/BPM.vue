<template>
    <!-- BPM changing -->
    <div class="container">
        <p>BPM</p>
        <!-- User will enter new desired BPM percentage here -->
        <input type="text" value="100%">
    </div>
</template>

<style scoped>
.container {
    border: solid 2px blue;

    display: flex;
    flex-direction: column;
    gap: 2px;

    max-width: 48%;
    height: 100%;

    padding: 1%;
}

input {
    max-width: 80%;
}
</style>