<template>
  <div class="text-to-speech">
    <div id="app">
  <transition name="fade" v-if="isLoading">
    <pulse-loader></pulse-loader>
  </transition>

  <transition name="fade" v-if="!isLoading">
    <div class="form-container">

      <form @submit.prevent="speak">
        <h1>Speech Example</h1>

        <div class="form-group" v-if="voiceList.length">
          <label for="voices">Select a voice</label>
          <select class="form-control" id="voices" v-model="selectedVoice">
            <option v-for="(voice, index) in voiceList" v-bind:key="index" 
            :data-lang="voice.lang" :value="index">
              {{ voice.name }} ({{ voice.lang }})
            </option>
          </select>
        </div>

        <div class="form-group">
          <label for="your-name">Your Text</label>
          <input class="form-control" id="your-name" type="text" v-model="text" required>
        </div>

        <button type="submit" class="btn btn-success">Speak</button>
      </form>

    </div>
  </transition>
</div>
  </div>
</template>

<script>
export default {
  name: 'TextToSpeach',
  props: {
    msg: String
  },
  data() {
    return {
      text: '',
      selectedVoice: 0,
      synth: window.speechSynthesis,
      voiceList: [],
      greetingSpeech: new window.SpeechSynthesisUtterance()
    }
  },
  mounted() {
    // wait for voices to load
    // I can't get FF to work without calling this first
    // Chrome works on the onvoiceschanged function
    this.voiceList = this.synth.getVoices()

    if (this.voiceList.length) {
      this.isLoading = false
    }

    this.synth.onvoiceschanged = () => {
      this.voiceList = this.synth.getVoices()
      // give a bit of delay to show loading screen
      // just for the sake of it, I suppose. Not the best reason
      setTimeout(() => {
        this.isLoading = false
      }, 800)
    }

    this.listenForSpeechEvents()
  },
  methods: {
    /**
     * React to speech events
     */
    listenForSpeechEvents () {
      this.greetingSpeech.onstart = () => {
        this.isLoading = true
      }

      this.greetingSpeech.onend = () => {
        this.isLoading = false
      }
    },

    /**
     * Shout at the user
     */
    speak () {
      // it should be 'craic', but it doesn't sound right
      this.greetingSpeech.text = this.text;

      this.greetingSpeech.voice = this.voiceList[this.selectedVoice];
      
      
      this.synth.speak(this.greetingSpeech);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
