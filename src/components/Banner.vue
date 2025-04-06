<template>
    <!-- Welcome message and PPJT logo -->
    <div class="banner">
        <h1 class="title">Welcome!</h1>
        <img class="logo" src="./images/ppjt_logo_full.jpg" alt="PPJT Logo">
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
        max-height: 144px;
        margin-left: auto;
        margin-right: 2%;
    }

    /* Colors */
    .banner {
        background-color: var(--color-bg);
    }
    
    img {
        border: 1px solid var(--color-border);
    }

    h1 {
        color: var(--color-text-main);
    }
</style>