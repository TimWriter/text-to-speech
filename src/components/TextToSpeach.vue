<template>
  <div class="text-to-speech">
    <div id="app">
    <div class="form-container"  v-bind:class="{ active: localTranslate.output !== '', hidden: localTranslate.output == '' }">
      <form @submit.prevent="speak">
        <span v-if="localTranslate.to == 'de-DE'">
        German
        </span>
        <span v-if="localTranslate.to == 'en-US'">
          English
        </span>
        <div>
          {{localTranslate.output}}
        </div>
        <button type="submit" class="btn btn-success">
          <i class="fas fa-volume-up"></i>
        </button>
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
.form-container{
  position: absolute;
  left: 50%;
  bottom: 30vh;
  background-color:  #4085F4;
  font-family: 'Roboto', sans-serif;
  width: 90vw;
  height: 30vh;
  transform: translateX(-50%);
  border-radius: 20px;
  
  span{
    position: absolute;
    left: 2em;
    top: 1em;
    font-size: 1.2em;
    color: #fff;
    font-weight: bold;
  }

  div{
    position: absolute;
    left: 2em;
    top: 3em;
    font-size: 1.2em;
    color: #fff;
  }

  button{
    position: absolute;
    right: 2em;
    top: 1em;
    appearance: none;
    outline: none;
    border: none;
    background-color:  #4085F4;
    color: white;
    font-size: 1.2em;
    cursor: pointer;

    &:hover{
      color: #ddd;
    }
  }
}

.hidden{
  opacity: 0;
}

.active{
  transition: .3s;
}
</style>
