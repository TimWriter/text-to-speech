<template>
  <div class="speech-rec">
    <button v-on:click="rec">Aufnehmen</button>
    {{localTranslate.input}}
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
        var that = this;

        recognition.onresult = function(event) {
            that.localTranslate.input = event.results[0][0].transcript;
        };

      },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
