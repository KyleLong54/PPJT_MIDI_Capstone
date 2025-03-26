<script setup>
// =================== Libraries ===================
import { useRouter, useRoute } from 'vue-router';
import { onMounted, ref } from 'vue';
import * as Tone from "tone";
import { Midi } from '@tonejs/midi';

// =================== Components ===================
import TimeContainer from '@/components/TimeContainer.vue';
import Logo from '@/components/Logo.vue';
import ButtonContainer from '@/components/ButtonContainer.vue';
import SongContainer from '@/components/SongContainer.vue';
import ExitButton from '@/components/ExitButton.vue';
import AddLabelForm from '@/components/AddLabelForm.vue';

// =================== Models ===================
import { LabelModel } from '@/models/LabelModel';

const synths = [];
let startTicks = 0;
let endTick = -1;

// Get Router and Route
const router = useRouter();
const route = useRoute();

// Get reference to the song container for label creation
const songContainer = ref(null);

// -- Called when view is mounted --
// Loads song into playable format in memory
onMounted(() => {
  // Get the midi file from the previous page
  let midi = getFile();

  // If the midi file was found
  if (midi) {
    midi.tracks.forEach(track => {
      // Create a synth for each track
      let synth = new Tone.PolySynth(Tone.Synth).toDestination();

      // Add all notes in the track to be played
      if (track.notes.length > 0) {
        const part = new Tone.Part((time, note) => {
          synth.triggerAttackRelease(note.name, note.duration, time, note.velocity)
        }, track.notes).start(0);
      } // end if
    }) // end forEach
  } // end if
}); // end onMounted

// Gets the uploaded MIDI file from the Session Storage
// return -> the MIDI file found from Session Storage
const getFile = () => {
  // Pares the midi object from the previous page
  let midi = JSON.parse(sessionStorage.getItem("MidiJSON"));
  console.log(midi);

  return midi;
} // end getFile

// Begins the playback at the current time
const play = async () => {
  await Tone.start();
  Tone.getTransport().start();
} // end play

// Exits the page, returning to home
const exit = () => {
  // Clean up Tone Transport
  Tone.getTransport().dispose();

  // Navigate to the home page
  router.push("/");
} // end exit

// Pauses the current playback, can be played later
const pause = () => {
  Tone.getTransport().pause();
} // end pause

// Stops the current playback, can be played later, but will be restarted
const stop = () => {
  Tone.getTransport().stop();
} // end stop

// Skips the playback to the given measure
const skip = (measureNum) => {
  stop();

  console.log("Starting playback from measure #" + measureNum);
  Tone.getTransport().start("+0", measureNum + ":0:0");
} // end skip

// Test method -> unused
const altSplit = () => {
  let furthestTick = -1;

  let midi = getFile();

  midi.tracks.forEach((track) => {
    let testTick = track.notes[track.notes.length - 1].ticks + track.notes[track.notes.length - 1].durationTicks;

    if (testTick > furthestTick) {
      furthestTick = testTick;
    }
  })

  console.log(Tone.Ticks(furthestTick).toBarsBeatsSixteenths());
}

// Splits uploaded song into measures (for measure displaying)
const split = () => {
  // Get midi file
  let midi = getFile();

  let measureSizes = [];

  // Get the length of each measure
  for (let i = 0; i < midi.header.timeSignatures.length; i++) {
    // Calculate the current measure size
    let measureSize = ((4 / midi.header.timeSignatures[i].timeSignature[1]) * midi.header.timeSignatures[i].timeSignature[0]) * midi.header.ppq;
    // console.log(measureSize);

    measureSizes.push(measureSize);
  } // end for

  console.log("Measure Sizes: " + measureSizes)

  // Furthest tick in the track
  let furthestTick = findEndTick();

  // Variable to store current tick during scan
  let currentTick = 0;

  // Measures in song
  let measures = [];

  for (let i = 0; i < midi.header.timeSignatures.length; i++) {
    console.log("On setting measure sizes iteration:" + i);

    let marginTick = -1;

    if (i + 1 == midi.header.timeSignatures.length - 1) {
      marginTick = midi.header.timeSignatures[i + 1].ticks;
    } else {
      marginTick = furthestTick;
    } // end if

    let index = 0;

    console.log("The current measure size is " + measureSizes[i] + " and the current margin is " + marginTick + ". We are at tick " + currentTick);

    while (currentTick < marginTick) {
      measures.push(measureSizes[i]);

      currentTick += measureSizes[i];
    } // end while
  } // end for

  console.log("Measures" + measures + ". We have " + measures.length + " measures in the song.");

  return measures;
} // end split

// Finds the furthest ending tick of a track in the song 
const findEndTick = () => {
  let furthestTick = -1;
  let midi = getFile();

  // Finds the furthest ending tick on a track
  midi.tracks.forEach((track) => {
    if (furthestTick < track.endOfTrackTicks) {
      furthestTick = track.endOfTrackTicks;
    } // end if
  }); // end forEach

  return furthestTick;
} // end findEndTick

// Adds a label to the label tray
const addLabel = (labelToAdd) => {
  songContainer.value.addLabel(labelToAdd);
} // end addLabel
</script>

<template>
  <header>
  </header>
  <main>
    <!--Contains playback and time controls-->
    <div class="row">
      <ButtonContainer style="height: 100%;" @play="play" @pause="pause" @stop="stop" />
      <TimeContainer style="height: 100%;" />
      <Logo />
    </div>

    <!--Contains song measure data-->
    <div class="measuresRow">
      <SongContainer ref="songContainer" :Title="route.query.Title" :Labels="null" :-mesaures="split()" @skip="skip" />
    </div>

    <!--Contains the label creation-->
    <div class="labelRow">
      <AddLabelForm @AddLabel="addLabel" :-measures="split()"></AddLabelForm>
    </div>

    <!--Contains the exit button-->
    <div class="lastRow">
      <ExitButton :Title="$route.query.Title" @Exit="exit"></ExitButton>
    </div>

    <!-- Test Button (for testing methods during development) -->
    <!-- <button v-on:click="addLabel">TEST BUTTON</button> -->
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
  line-height: 1.5;
}

.row {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-evenly;
  width: 100%;

  gap: 20px;
}

.measuresRow {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-evenly;
  width: 100%;

  gap: 20px;

  padding-bottom: 2%;
}

.labelRow {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: left;
  width: 100%;

  gap: 20px;

  padding-bottom: 3%;
  padding-left: .5%;
}

.lastRow {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: right;
  width: 100%;

  gap: 20px;
}
</style>