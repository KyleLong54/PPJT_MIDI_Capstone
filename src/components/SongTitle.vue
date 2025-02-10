<script setup>
defineProps({
    Title: String
}) // end defineProps
</script>

<template>
    <div class="aligner">
        <p>{{ Title }}</p>
    </div>
</template>

<style scoped>
p {
    max-width: 80%;
    border: 1px solid black;
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
</style>

<script>
export default {

} // end export
</script>