<template>
  <div class="home">
    {{translate}}
    <SpeechRecognition v-bind:translate="translate" />
    <TextToSpeach v-bind:translate="translate" />
    <button v-on:click="changeLanguages">Change</button>
    <button v-on:click="call">Translate</button>
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
  methods: {
    changeLanguages () {
      let oldFrom = this.translate.from;
      this.translate.from = this.translate.to;
      this.translate.to = oldFrom;
    },
    call () {
      var session_url = 'https://api.eu-de.language-translator.watson.cloud.ibm.com/instances/b0a1d289-374a-40dd-b5e6-949abbc53ef5/v3/translate?version=2018-05-01';
      var uname = 'apikey';
      var pass = 'F8sX9zFhk9RcOhyw6HLiLe1mon_RQv_f5b7yDMhh3BOH';
      var textArray = [];
      textArray.push(this.translate.input);
      var translation = (this.translate.from.substring(0,3)) + this.translate.to.substring(0,2);
      var that = this;
      axios.post(
        session_url,
        {
          headers: {
            "Content-Type": "application/json",
          },
          text: textArray,
          model_id: translation,
        },
        {
          auth: {
            username: uname,
            password: pass
        }
      }).then(function(response) {
        that.translate.output = response.data.translations[0].translation;
      }).catch(function(error) {
        console.log(error);
      });
    }
  }
}
</script>
