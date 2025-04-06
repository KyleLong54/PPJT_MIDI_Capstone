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

  font-family: 'Montserrat', sans-serif;
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

  font-family: 'Montserrat', sans-serif;
}

@font-face {
  font-family: 'Montserrat';
  src: url('./assets/fonts/Poppins-Regular.ttf') format('ttf');
  font-weight: 400;
  font-style: normal;
}

.uploadContainer {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* Colors */
header {
  border-bottom: 2px solid var(--color-accent-ppjt-green);
}

button {
  background-color: var(--color-button-bg);
}

button:hover {
  cursor: pointer;
  background-color: var(--color-button-hover);
}

img {
  border: 1px solid var(--color-border);
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