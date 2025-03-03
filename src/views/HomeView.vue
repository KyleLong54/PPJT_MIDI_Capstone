<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Banner from '../components/Banner.vue'
import UploadContent from '../components/UploadContent.vue';
import UploadError from '@/components/UploadError.vue';
</script>

<template>
  <header>
    <Banner/>
  </header>
  <main>
    <UploadContent @show-error-message="showErrorMessage"/>
    <UploadError id="upload_error" style="visibility: hidden;"/>
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

<script>
export default {
  methods: {
    showErrorMessage() {
      document.getElementById('upload_error').style.visibility = 'visible';
    }
  } // end methods
} // end export default
</script>