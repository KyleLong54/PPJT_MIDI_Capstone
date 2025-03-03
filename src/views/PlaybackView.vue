<script setup>
// =================== Libraries ===================
import { useRouter, useRoute } from 'vue-router'
import { onMounted} from 'vue'
import * as Tone from "tone"
import { Midi } from '@tonejs/midi'

// =================== Components ===================
import TimeContainer from '@/components/TimeContainer.vue'
import Logo from '@/components/Logo.vue'
import ButtonContainer from '@/components/ButtonContainer.vue'
import SongContainer from '@/components/SongContainer.vue'
import ExitButton from '@/components/ExitButton.vue'
import AddLabelForm from '@/components/AddLabelForm.vue'

// =================== Models ===================
import { LabelModel } from '@/models/LabelModel'

// =================== Functions ===================

const synths = [];
let startTicks = 0;
let endTick = -1;

// Get Router and Route
const router = useRouter();
const route = useRoute();

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
const play = () => {
  // Get the MIDI file from the session storage
  let midi = getFile();

  // If we found the file
  if (midi) {
    const now = Tone.now() + 0.5;

    midi.tracks.forEach((track) => {
      //create a synth for each track
      const synth = new Tone.PolySynth(Tone.Synth, {
        envelope: {
          attack: 0.02,
          decay: 0.1,
          sustain: 0.3,
          release: 1,
        },
      }).toDestination()

      // Add the synth to the array
      synths.push(synth)

      // Find all of the notes that are after the start point
      let skipNotes = getNotes(track, startTicks)

      // Schedule each note to be played
      skipNotes.forEach((note) => {
        synth.triggerAttackRelease(
          note.name,
          note.duration,
          note.time + now,
          note.velocity
        ) // end triggerAttackRelease
      }) // end foreach
    }) // end forEach
  }
  // Otherwise
  else {
    // Remove the synths
    removeSynths();
  } // end if

} // end play

const altPlay = async () => {
  await Tone.start();
  Tone.getTransport().start();
} // end altPlay

// Removes all synths 
const removeSynths = () => {
  // Removes each synths from the array
  while (synths.length) {
    const synth = synths.shift();
    synth.disconnect();
  } // end while
} // end removeSynths

// Exits the page, returning to home
const exit = () => {
  // Clean up Tone Transport
  Tone.getTransport().dispose();

  // Navigate to the home page
  router.push("/");
} // end exit

// Gets all of the notes that are after the given point
// track -> the track from the MIDI file to find the notes for
// ticks -> the tick number to find notes at
// return -> all notes in the track that are at or after the given tick number 
const getNotes = (track, ticks) => {
  // Find the starting index of the first note at or after the given tick
  let index = track.notes.findIndex((note) => {
    return note.ticks >= ticks;
  });

  // Get all notes to be played
  let allNotes = track.notes.slice(index, track.notes.length)

  // For time syncing, subtract the starting time from each note
  for (let note of allNotes) {
    note.ticks = note.ticks - ticks;
  } // end for

  return allNotes;
} // end getNotes

// Begins the MIDI playback at the beginning of the track
const start = () => {
  play();
} // end start

const skip = () => {

} // end skip

// Pauses the current playback, can be played later
// NOT FUNCTIONAL NOW
const pause = () => {
  startTicks = Tone.Time(Tone.now() + 0.5).toTicks();
  console.log(startTicks);
  removeSynths();
} // end pause

const altPause = () => {
  Tone.getTransport().pause();
} // end altPause

// Stops the current playback, can be played later, but will be restarted
const stop = () => {
  startTicks = 0;
  removeSynths();
}

const altStop = () => {
  Tone.getTransport().stop();
  Tone.getTransport().dispose();
  removeSynths();
} // end altStop

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

const findEndTick = () => {
  let furthestTick = -1;
  let midi = getFile();

  midi.tracks.forEach((track) => {
    if (furthestTick < track.endOfTrackTicks) {
      furthestTick = track.endOfTrackTicks;
    } // end if
  }); // end forEach

  console.log(furthestTick);
  return furthestTick;
} // end findEndTick

</script>

<template>
  <header>
  </header>
  <main>
    <!--Contains playback and time controls-->
    <div class="row">
      <ButtonContainer style="height: 100%;" @play="altPlay" @pause="altPause" @stop="altStop" />
      <TimeContainer style="height: 100%;" />
      <Logo />
    </div>

    <!--Contains song measure data-->
    <div class="measuresRow">
      <SongContainer :Title="route.query.Title" :Labels="split()" />
    </div>

    <!--Contains the label creation-->
    <div class="labelRow">
      <AddLabelForm @AddLabel=""></AddLabelForm>
    </div>

    <!--Contains the exit button-->
    <div class="lastRow">
      <ExitButton :Title="$route.query.Title" @Exit="exit"></ExitButton>
    </div>

    <button v-on:click="split">TEST BUTTON</button>
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