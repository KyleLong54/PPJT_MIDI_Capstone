<script setup>
import AddTitle from './AddTitle.vue';
</script>

<template>
    <div class="container">
        <AddTitle></AddTitle>
        <div class="form">
            <div class="addTitle">
                <p>Title:</p>
                <input type="text" name="" id="">
            </div>
            <div class="selectMeasure">
                <p>Measure:</p>
                <select name="" id=""></select>
            </div>
            <div class="buttonContainer">
                <button v-on:click="test()">Create Label</button>
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

<script>
export default {
    methods: {
        test() {
            this.$emit("AddLabel", )
        }
    }
} // end export
</script>