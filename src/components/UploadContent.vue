<template>
  <form style="width: 100%;" class="container" action="">
    <input id="MIDI" class="uploadButton" @change="readFile" type="file" accept="audio/midi" >
  </form>
</template>

<style scoped>
.container {
  border: solid 3px green;
  position: relative;
  height: 50%;

  display: flex;
  justify-content: center;
  flex-direction: column;
}

.uploadButton {
  font-size: large;
}

.submitButton {
  font-size: large;
  width: 60%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 1.5em;
  margin-bottom: 1.5em;
}
</style>

<script>
import { Midi } from '@tonejs/midi';

export default {
  methods: {
    readFile() {
      let fileContent;

      if (document.getElementById("MIDI").files.length == 0) {
        alert("Please select a file to continue.")
      } else {
        const reader = new FileReader()

        reader.onload = function (e) {
          // JUST A PRINT
          console.log(e.target.result);
          // Create MIDI object using the uploaded file
          fileContent = new Midi(e.target.result)
          // Turn the object into JSON
          let midiJSON = JSON.stringify(fileContent, undefined, 2)
          // JUST A PRINT
          console.log(midiJSON);
          // SessionStorage the JSON to accesss on the next page
          sessionStorage.setItem("MidiJSON", midiJSON)
        } // end reader onload

        reader.onerror = function (e) {
          console.log(e);
        } // end reader onerror

        reader.readAsArrayBuffer(document.getElementById("MIDI").files[0])

        this.$router.push('/play')

      } // end if
    }, // end readFile
  },
};

</script>