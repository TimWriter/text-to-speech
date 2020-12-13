<template>
  <div class="home">
    <header>
      <div>
        <div v-if="translate.from == 'de-DE'">
        German
      </div>
      <div v-if="translate.from == 'en-US'">
        English
      </div>
      <button v-on:click="changeLanguages">
        <i class="fas fa-exchange-alt"></i>
      </button>
      <div v-if="translate.to == 'de-DE'">
        German
      </div>
      <div v-if="translate.to == 'en-US'">
        English
      </div>
      </div>
    </header>
    <main>
      <SpeechRecognition v-bind:translate="translate" />
      <TextToSpeach v-bind:translate="translate" />
      <button v-on:click="call">Translate</button>
    </main>
    
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
<style scoped lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;400;500;700&display=swap');


  header{
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 80px;
    background-color: #fff;
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-family: 'Roboto', sans-serif;
    overflow: hidden;

    *{
      display: inline-block;
      
    }

    button{
      appearance: none;
      border: none;
      background-color: #fff;
      color: #444;
      margin:  0 1em 0 1em;
      outline: none;
      cursor: pointer;
      width: 30px;
      height: 30px;
      border-radius: 15px;
      &:hover{
        background-color: #ddd;
      }
    }

    div{
      color: #4085F4;
      font-weight: bold;
    }
  }

  main{
    position: absolute;
    top: 80px;
    width: 100%;
    left: 0;
    overflow: hidden;

    button{
      position: absolute;
      bottom: 15vh;
      left: 50%;
      transform: translateX(-50%);
      width: 250px;
      height: 40px;
      border-radius: 20px;
      background-color: #4085F4;
      color: #fff;
      border: none;
      cursor: pointer;
      outline: none;
      font-family: 'Roboto', sans-serif;

      &:hover{
        background-color: #2e61b4;
      }
    }
  }
</style>
