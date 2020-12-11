<template>
  <div class="home">
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
