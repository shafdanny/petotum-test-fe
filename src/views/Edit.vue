<template>
  <div class="edit">
    <h2>Edit Mode</h2>
    <Box v-bind:editable="true"/>
  </div>
</template>
<script>
// @ is an alias to /src
import Box from '@/components/Box.vue'

export default {
  name: 'Home',
  components: {
    Box
  }
}
</script>