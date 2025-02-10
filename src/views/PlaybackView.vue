<script setup>
import { RouterLink, RouterView } from 'vue-router'
import * as Tone from "tone"
import { Midi } from '@tonejs/midi'
import TimeContainer from '@/components/TimeContainer.vue'
import Logo from '@/components/Logo.vue'
import ButtonContainer from '@/components/ButtonContainer.vue'
import SongContainer from '@/components/SongContainer.vue'
import ExitButton from '@/components/ExitButton.vue'
import AddLabelForm from '@/components/AddLabelForm.vue'
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
    <div class="row">
      <SongContainer :Title="$route.query.Title"/>
    </div>

    <!--Contains the label creation-->
    <div class="labelRow">
      <AddLabelForm></AddLabelForm>
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
  line-height: 2;
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

<script>
import { Midi } from '@tonejs/midi';
import { ref } from 'vue'
import ExitButton from '@/components/ExitButton.vue'

const synths = [];
let startTicks = 0;
let endTick = -1;

export default {
  mounted() {

  }, // end mounted
  methods: {
    // Gets the uploaded MIDI file from the Session Storage
    // return -> the MIDI file found from Session Storage
    getFile() {
      let midi = JSON.parse(sessionStorage.getItem("MidiJSON"));
      console.log(midi);

      return midi;
    }, // end getFile

    // Begins the playback at the current time
    play() {
      // Get the MIDI file from the session storage
      let midi = this.getFile();

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
          let skipNotes = this.getNotes(track, startTicks)

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
        this.removeSynths();
      } // end if

    }, // end play

    // Removes all synths 
    removeSynths() {
      // Removes each synths from the array
      while (synths.length) {
        const synth = synths.shift();
        synth.disconnect();
      } // end while
    }, // end removeSynths

    // Exits the page, returning to home
    exit() {
      // Remove synths to stop playback
      this.removeSynths();

      // Navigate to the home page
      this.$router.push('/')
    }, // end exit

    // Gets all of the notes that are after the given point
    // track -> the track from the MIDI file to find the notes for
    // ticks -> the tick number to find notes at
    // return -> all notes in the track that are at or after the given tick number 
    getNotes(track, ticks) {
      // Find the starting index of the first note at or after the given tick
      let index = track.notes.findIndex((note) => {
        return note.ticks >= ticks;
      });

      return track.notes.slice(index, track.notes.length);
    }, // end getNotes

    // Begins the MIDI playback at the beginning of the track
    start() {
      this.play();
    },

    skip() {

    },

    // Pauses the current playback, can be played later
    pause() {
      startTicks = Tone.Time(Tone.now() + 0.5).toTicks();
      console.log(startTicks);
      this.removeSynths();
    },

    // Stops the current playback, can be played later, but will be restarted
    stop() {
      startTicks = 0;
      this.removeSynths();
    },

    split() {
      // Get midi file
      let midi = this.getFile();

      console.log(midi.header.ppq);
      console.log(midi.header.tempos);
      console.log(midi.header.timeSignatures)

      let measureSizes = [];

      // Get the length of each measure
      for (let i = 0; i < midi.header.timeSignatures.length; i++) {
        // Calculate the current measure size
        let measureSize = ((4 / midi.header.timeSignatures[i].timeSignature[1]) * midi.header.timeSignatures[i].timeSignature[0]) * midi.header.ppq;
        console.log(measureSize);

        measureSizes.push(measureSize);
      } // end for

      let furthestTick = this.findEndTick();

      let currentTick = 0;

    }, // end split

    findEndTick() {
      let furthestTick = -1;
      let midi = this.getFile();

      midi.tracks.forEach((track) => {
        if (furthestTick > track.notes[track.notes.length - 1].ticks + track.notes[track.notes.length - 1].length) {
          furthestTick = track.notes[track.notes.length - 1].ticks + track.notes[track.notes.length - 1].length;
        } // end if
      }); // end forEach

      console.log(furthestTick);
      return furthestTick;
    }, // end findEndTick
  } // end methods
}; // end export
</script>