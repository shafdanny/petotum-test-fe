<template>
  <v-app>
    <!-- <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar> -->

    <v-content>
      <Box v-bind:boxContents="boxContents"/>
    </v-content>
  </v-app>
</template>

<script>
import Box from './components/Box';

export default {
  name: 'App',
  components: {
    Box,
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
};
</script>
<style scoped>
#app {
  margin: 40px 0 0;
}
</style>
