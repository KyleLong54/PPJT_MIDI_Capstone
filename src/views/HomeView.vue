<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Banner from '../components/Banner.vue'
import UploadContent from '../components/UploadContent.vue';
</script>

<template>
  <header>
    <Banner/>
  </header>
  <main>
    <UploadContent/>
  </main>
</template>

<style scoped>
header {
  line-height: 3;
  width: 100%;
}
main {
  height: 100%;
  width: 100%;
  line-height: 5;
}
</style>