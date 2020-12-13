<template>
  <div class="text-to-speech">
    <div id="app">

  
    <div class="form-container">

      <form @submit.prevent="speak">
        <h3>{{localTranslate.output}}</h3>

        <button type="submit" class="btn btn-success">Abspielen</button>
      </form>

    </div>
  
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

    this.voiceList = this.synth.getVoices()

    if (this.voiceList.length) {
      this.isLoading = false
    }

    this.synth.onvoiceschanged = () => {
      this.voiceList = this.synth.getVoices()
    
      setTimeout(() => {
        this.isLoading = false
      }, 800)
    }

    this.listenForSpeechEvents()
  },
  methods: {
  
    listenForSpeechEvents () {
      this.greetingSpeech.onstart = () => {
        this.isLoading = true
      }

      this.greetingSpeech.onend = () => {
        this.isLoading = false
      }
    },

  
    speak () {
      this.greetingSpeech.text = this.localTranslate.output;

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
