<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <TextToSpeach />
  </div>
</template>

<script>
// @ is an alias to /src
import TextToSpeach from '@/components/TextToSpeach.vue'

export default {
  name: 'Home',
  components: {
    TextToSpeach,
  }
}
</script>
