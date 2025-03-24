<template>
    <!-- Time Signature changing -->
    <div class="container">
        <p>Time Signature</p>
        <!-- User will enter new desired time signature here -->
        <input type="text" value="4 | 4">
    </div>
</template>

<style scoped>
.container {
    border: solid 2px red;

    display: flex;
    flex-direction: column;
    gap: 2px;

    max-width: 48%;
}

input {
    max-width: 80%;
}
</style>