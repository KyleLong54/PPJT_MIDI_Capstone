<script setup>
// =================== Libraries ===================
import { Midi } from '@tonejs/midi';
import { ref } from 'vue';
import { useRouter, useRoute } from 'vue-router'

// Get router and route
const router = useRouter();
const route = useRoute();

// Song title to be sent to playback page
let songTitle = "";

// Define Emits
const emit = defineEmits(['show-error-message'])

// Reference to the file upload
const uploadInput = ref(null);

// Reads the uploaded file
const readFile = () => {
  let fileContent;
      if (uploadInput.value.files.length == 0) {
        alert("Please select a file to continue.");
      } else {
        const reader = new FileReader()

        reader.onload = function (e) {
          // Get size of the file
          let filesize = uploadInput.value.files[0].size / 1024;
          console.log(filesize);

          // JUST A PRINT
          console.log(e.target.result);

          // Save file name for song title
          songTitle = uploadInput.value.files[0].name;
          songTitle = songTitle.split('.')[0];

          // Check to see if the file is too large
          if (filesize > 50) {
            // Show error message
            emit('show-error-message', true);
          } else {
            // Create MIDI object using the uploaded file
            fileContent = new Midi(e.target.result)
            console.log(fileContent);
            // Turn the object into JSON
            let midiJSON = JSON.stringify(fileContent, undefined, 2);
            // JUST A PRINT
            console.log(midiJSON);
            // SessionStorage the JSON to accesss on the next page
            sessionStorage.setItem("MidiJSON", midiJSON);

            // Load page
            router.push({path: '/play', query: { Title: songTitle } });

            // Make sure that error message is not showing
            emit('show-error-message', false);
          } // end if
        } // end reader onload

        reader.onerror = function (e) {
          console.log(e);
        } // end reader onerror

        reader.readAsArrayBuffer(uploadInput.value.files[0])
      } // end if
} // end readFile

// Function to facilitate file uploading
const fileUpload = () => {
  uploadInput.value.click();
} // end fileUpload
</script>

<template>
  <form style="width: 100%;" class="container" action="">
    <!-- Button that causes file upload -->
    <button class="uploadButton" type="button" v-on:click="fileUpload">Upload File</button>
    <!-- Hidden upload file input (used when the button is clicked) -->
    <input ref="uploadInput" style="visibility: hidden;" id="MIDI" class="uploadButton" @change="readFile" type="file" accept="audio/midi">
  </form>
</template>

<style scoped>
.container {
  position: relative;
  height: 50%;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.uploadButton {
  font-size: large;
  width: 75%;
}
</style>