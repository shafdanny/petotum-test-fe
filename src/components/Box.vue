<template>
  <v-container class="grey lighten-5">
    <v-row v-for="rowIndex in 4" :key="rowIndex">
      <v-col v-for="n in 4" :key="n" cols="12" md="3">
        <v-card
          class="card rounded-lg pa-4"
          outlined
          tile
          v-if="getBoxContent(getPosition(rowIndex, n))"
        >{{ getBoxContent(getPosition(rowIndex, n)).text }}</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Box",
  props: ["boxContents"],
  methods: {
    getBoxContent: function(position) {
      console.log("Getting box content in position " + position);
      return this.boxContents.find(element => element.position == position);
    },
    getPosition: function(rowIndex, colIndex) {
      return (rowIndex - 1) * 4 + colIndex;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  padding: 1em;
  text-align: center;
}

</style>
