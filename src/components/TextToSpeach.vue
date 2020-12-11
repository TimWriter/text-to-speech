<template>
  <div class="text-to-speech">
    <div id="app">

  <transition name="fade" v-if="!isLoading">
    <div class="form-container">

      <form @submit.prevent="speak">
        <h3>{{localTranslate.output}}</h3>

        <button type="submit" class="btn btn-success">Abspielen</button>
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
    translate: {
      type: Object,
    },
  },
  data() {
    return {
      localTranslate: '',
      isLoading: true,
      text: '',
      selectedVoice: 0,
      synth: window.speechSynthesis,
      voiceList: [],
      greetingSpeech: new window.SpeechSynthesisUtterance()
    }
  },
  mounted() {
    
    this.localTranslate = this.translate;

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

      if(this.localTranslate.to == 'de-DE'){
        this.greetingSpeech.voice = this.voiceList[2];
      }
      if(this.localTranslate.to == 'en-US'){
        this.greetingSpeech.voice = this.voiceList[3];
      }

      this.synth.speak(this.greetingSpeech);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
