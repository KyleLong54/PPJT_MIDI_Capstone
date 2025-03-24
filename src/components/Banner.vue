<template>
    <!-- Welcome message and PPJT logo -->
    <div class="banner">
        <h1 class="title">Welcome!</h1>
        <img class="logo" src="./images/ppjt_logo.jpg" alt="PPJT Logo">
    </div>
</template>

<style scoped>
    .banner {
        /* Change these as needed to change the background */
        /* background-position: center;
        background-repeat: no-repeat;
        background-color: blue; */

        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        align-items: center;
        justify-content: center;
        width: 100%;
    }

    .title {
        font-size: 50px;
        text-align: center;
        padding: 1%;
        flex-grow: 1;
    }

    .logo {
        max-width: 180px;
        max-height: 172px;
        margin-left: auto;
        padding: 1%;
    }
</style>