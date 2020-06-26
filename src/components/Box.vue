<template>
  <v-container class="lighten-5">
    <div class="row" v-if="editable">
      <div class="col-2">
        <v-btn small block color="normal" @click="originalOrder">To original order</v-btn>
      </div>
      <div class="col-6"></div>
      <div class="col-2">
        <v-btn small block color="error" @click="sort">Cancel</v-btn>
      </div>
      <div class="col-2">
        <v-btn small block color="primary" @click="save">Save</v-btn>
      </div>
    </div>
    <v-container class="grey lighten-5">
      <div class="col-2" v-if="editable"></div>
      <draggable class="row" :list="localBoxContents" v-if="editable" @end="log">
        <v-col v-for="boxContent in localBoxContents" :key="boxContent.position" cols="12" md="3">
          <v-card
            class="card rounded-lg pa-4"
            outlined
            tile
            v-if="boxContent.active"
          >{{ boxContent.text }}</v-card>
          <v-card v-else class="card empty-card pa-4">Empty</v-card>
        </v-col>
      </draggable>
      <v-row class="row" v-else>
        <v-col v-for="boxContent in localBoxContents" :key="boxContent.position" cols="12" md="3">
          <v-card
            class="card rounded-lg pa-4"
            outlined
            tile
            v-if="boxContent.active"
          >{{ boxContent.text }}</v-card>
          <v-card v-else class="card empty-card pa-4">Empty</v-card>
        </v-col>
      </v-row>
    </v-container>
    <v-alert transition="slide-x-transition" v-model="showAlert" :dismissible="true" type="success">
      Block component updated
    </v-alert>
  </v-container>
</template>

<script>
import draggable from "vuedraggable";
import axios from "axios";

export default {
  name: "Box",
  props: ["editable"],
  components: {
    draggable
  },
  data() {
    return {
      localBoxContents: [],
      showAlert: false
    };
  },
  async mounted() {
    let response = await axios.get(`${process.env.VUE_APP_API_BASE_URL}/blocks`);
    console.log(response.data)
    this.localBoxContents = response.data;
    console.log("API BASE URL: " + process.env.VUE_APP_API_BASE_URL)
  },
  methods: {
    generateLocalBoxContents() {
      let newArrayContent = this.boxContents.map(a => ({ ...a }));

      return newArrayContent;
    },
    getBoxContent: function(position) {
      return this.boxContents.find(element => element.position == position);
    },
    getPosition: function(rowIndex) {
      return rowIndex; //(rowIndex - 1) * 4 + colIndex;
    },
    log: function() {
      console.log(JSON.stringify(this.localBoxContents));
    },
    sort: function() {
      this.localBoxContents = this.localBoxContents.sort(
        (a, b) => a.position - b.position
      );
    },
    originalOrder: function() {
      this.localBoxContents.splice(0, this.localBoxContents.length) 
      this.localBoxContents = [
        { position: 1, text: "", active: false },
        { position: 2, text: "petotum.com", active: true },
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
        { position: 16, text: "Provide Transparency", active: true }
      ];
      this.sort();
    },
    save: async function() {
      for (let i = 0; i < this.localBoxContents.length; i++) {
        this.localBoxContents[i].position = i + 1;
      }
      console.log(JSON.stringify(this.localBoxContents));
      
      await axios.put(`${process.env.VUE_APP_API_BASE_URL}/blocks`, this.localBoxContents);
      this.showAlert = true;
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

.empty-card {
  visibility: hidden;
}
</style>
