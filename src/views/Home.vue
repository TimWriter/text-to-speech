<template>
  <div class="home">
    {{translate}}
    <SpeechRecognition v-bind:translate="translate" />
    <TextToSpeach v-bind:translate="translate" />
    <button v-on:click="changeLanguages">Change</button>
  </div>
</template>

<script>
import axios from 'axios';
// @ is an alias to /src
import TextToSpeach from '@/components/TextToSpeach.vue'
import SpeechRecognition from '@/components/SpeechRecognition.vue'

export default {
  name: 'Home',
  components: {
    TextToSpeach,
    SpeechRecognition,
  },
  data() {
    return {
      translate: {
        from: 'de-DE',
        to: 'en-US',
        input: '',
        output: '',
      },
      apiKey: 'F8sX9zFhk9RcOhyw6HLiLe1mon_RQv_f5b7yDMhh3BOH',
    }
  },
  mounted() {
    axios
      .get(`https://api.eu-de.language-translator.watson.cloud.ibm.com/instances/b0a1d289-374a-40dd-b5e6-949abbc53ef5`)
      .then((response) => { this.user = response.data; });
  },
  methods: {
    changeLanguages () {
      let oldFrom = this.translate.from;
      this.translate.from = this.translate.to;
      this.translate.to = oldFrom;
    }
  }
}
</script>
