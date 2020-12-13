<template>
  <div class="speech-rec">
    <div v-if="isListening" class="listening">
      <div>
        <span></span>
        <span></span>
        <span></span>
      </div>
    </div>
    <textarea type="text" v-model="localTranslate.input" placeholder="Your text here" />
    <button v-on:click="rec">
      <i class="fas fa-microphone"></i>
    </button>
  </div>
</template>

<script>
export default {
  name: 'SpechRecognition',
  props: {
    translate: {
      type: Object,
    },
  },
  data() {
    return {
      localTranslate: '',
      isListening: false,
    }
  },
  mounted() {
      this.localTranslate = this.translate;
  },
  methods: {
      rec () {
        var recognition = new window.webkitSpeechRecognition;
        recognition.lang = this.localTranslate.from;
        recognition.interimResults = false;
        recognition.maxAlternatives = 5;
        recognition.start();
        this.isListening = true;
        var that = this;

        recognition.onresult = function(event) {
            that.isListening = false;
            that.localTranslate.input = event.results[0][0].transcript;
        };

      },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
textarea{
  width: 100vw;
  height: calc(100vh - 80px);
  border: none;
  outline: none;
  background-color: #eee;
  padding: none;
  font-size: 1.5em;
  padding: 1em 4em 1em 4em;
  font-family: 'Roboto', sans-serif;
}

button{
  position: absolute;
  top: 20px;
  right: 100px;
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 25px;
  background-color:  #4085F4;
  font-size: 1.2em;
  color: #fff;
  cursor: pointer;
  outline: none;
  &:hover{
        background-color: #2e61b4;
      }
}

.listening{
  position: absolute;
  left: 0;
  top: 0;
  width: 100vw;
  height: calc(100vh - 80px);
  z-index: 10;
  background-color: rgba(255, 255, 255, 0.6);

  div{
    position: absolute;
    left: 50%;
    top: 40%;
    width: 90px;
    height: 20px;
    transform: translate(-50%, -50%);

    span{
      position: absolute;
      width: 20px;
      height: 20px;
      border-radius: 10px;
      background-color:  #4085F4;

      &:nth-child(1){
        left: 0;
          animation-name: recording;
          animation-duration: 1s;
          animation-delay: 0s;
          animation-iteration-count: infinite;
      }

      &:nth-child(2){
        left: 30px;
        animation-name: recording;
          animation-duration: 1s;
          animation-delay: .3s;
          animation-iteration-count: infinite;
      }

      &:nth-child(3){
        left: 60px;
        animation-name: recording;
          animation-duration: 1s;
          animation-delay: .6s;
          animation-iteration-count: infinite;
      }
    }
  }
}

@keyframes recording {
  0%   {transform: translateY(0px);}
  50%  {transform: translateY(-20px);}
  0%   {transform: translateY(0px);}
}
</style>
