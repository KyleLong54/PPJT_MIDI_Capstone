<script setup>
// =================== Libraries ===================
import { RouterLink, RouterView } from 'vue-router'

// =================== Components ===================
import Banner from '../components/Banner.vue'
import UploadContent from '../components/UploadContent.vue';
import UploadError from '@/components/UploadError.vue';
</script>

<template>
  <!-- Banner with title and logo -->
  <header>
    <Banner />
  </header>

  <main>
    <!-- Upload button -->
    <UploadContent @show-error-message="showErrorMessage" />
    <!-- Upload error message (only shown when there is an error with file upload) -->
    <UploadError id="upload_error" style="visibility: hidden;" />
  </main>
</template>

<style scoped>
header {
  line-height: 3;
  width: 100%;

  border-bottom: 2px black solid;
}

main {
  height: 100%;
  width: 100%;
  line-height: 5;

  margin-top: 5%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.uploadContainer {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
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