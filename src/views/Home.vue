<template>
  <div class="home">
    <Box v-bind:boxContents="boxContents"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Box from '@/components/Box.vue'

export default {
  name: 'Home',
  components: {
    Box
  },
  data: () => ({
    boxContents: [
      { position:2, text: "petotum.com" },
      { position:4, text: "Build Trust" },
      { position:7, text: "Pet care dashboard" },
      { position:9, text: "Pet Parent Dashboard" },
      { position:16, text: "Provide Transparency" },
    ]
  }),
}
</script>
