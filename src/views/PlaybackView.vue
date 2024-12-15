<script setup>
import { RouterLink, RouterView } from 'vue-router'
import * as Tone from "tone"
import { Midi } from '@tonejs/midi'
import TimeContainer from '../components/TimeContainer.vue'
import Logo from '../components/Logo.vue'
import ButtonContainer from '../components/ButtonContainer.vue'
</script>

<template>
  <header>
  </header>
  <main>
    <form action="/">
      <input @click="removeSynths" type="submit" value="Back to Homepage">
    </form>

    <button v-on:click="start">Play</button>
    <button v-on:click="stop">Stop</button>
    <button v-on:click="pause">Pause</button>

    <div class="rowOne">
      <ButtonContainer style="height: 100%;"/>
      <TimeContainer style="height: 100%;"/>
      <Logo />
    </div>


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

.rowOne {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-evenly;
  width: 100%;

  gap: 20px;

  border: solid 2px black;
}
</style>

<script>
import { Midi } from '@tonejs/midi';
import { ref } from 'vue'

const synths = [];
let startTicks = 0;

export default {
  mounted() {

  }, // end mounted
  methods: {
    // Gets the uploaded MIDI file from the Session Storage
    // return -> the MIDI file found from Session Storage
    getFile() {
      let midi = JSON.parse(sessionStorage.getItem("MidiJSON"))
      console.log(midi);

      return JSON.parse(sessionStorage.getItem("MidiJSON"))
    }, // end getFile

    // Begins the playback at a given time
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
      this.removeSynths();
    },
    // Stops the current playback, can be played later, but will be restarted
    stop() {
      startTicks = 0;
      this.removeSynths();
    },
  } // end methods
};

</script>