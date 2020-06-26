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
      { position: 1, text: "", active: false },
      { position: 2, text: "petotum.com", active: true  },
      { position: 3, text: "", active: false },
      { position: 4, text: "Build Trust", active: true },
      { position: 5, text: "", active: false },
      { position: 6, text: "", active: false },
      { position: 7, text: "Pet care dashboard", active: true },
      { position: 8, text: "", active: false },
      { position: 9, text: "Pet Parent Dashboard", active: true },
      { position: 10, text: "", active: false },
      { position: 11, text: "", active: false },
      { position: 12, text: "", active: false },
      { position: 13, text: "", active: false },
      { position: 14, text: "", active: false },
      { position: 15, text: "", active: false },
      { position: 16, text: "Provide Transparency", active: true},
    ]
  }),
}
</script>
