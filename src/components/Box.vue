<template>
  <v-container class="lighten-5">
    <div class="row" v-if="editable">
      <div class="col-2">
        <v-btn small block color="normal" @click="originalOrder">Reset to original</v-btn>
      </div>
      <div class="col-6"></div>
      <div class="col-2">
        <v-btn small block color="error" @click="cancel">Cancel</v-btn>
      </div>
      <div class="col-2">
        <v-btn small block color="primary" @click="save">Save</v-btn>
      </div>
    </div>
    <v-container class="grey lighten-5">
      <draggable class="row" :list="localBoxContents" v-if="editable" @end="log">
        <v-col v-for="boxContent in localBoxContents" :key="boxContent.position" cols="12" md="3">
          <v-card
            class="card rounded-lg pa-4"
            :color="boxContent.bg_color"
            tile
            hover
            v-if="boxContent.active"
          >
            <v-row no-gutters>
              <v-col v-html="boxContent.text"></v-col>
              <v-col v-if="editable" cols="2">
                <span class="group">
                  <v-icon
                    @click="editBlockText(boxContent.position, boxContent.text, boxContent.bg_color)"
                  >mdi-pencil-outline</v-icon>
                </span>
              </v-col>
            </v-row>
          </v-card>
          <v-card v-else class="card empty-card pa-4">Empty</v-card>
        </v-col>
      </draggable>
      <v-row class="row" v-else>
        <v-col v-for="boxContent in localBoxContents" :key="boxContent.position" cols="12" md="3">
          <v-card
            :color="boxContent.bg_color"
            class="card rounded-lg pa-4"
            tile
            hover
            v-if="boxContent.active"
          >
            <v-row no-gutters>
              <v-col v-html="boxContent.text"></v-col>
            </v-row>
          </v-card>
          <v-card v-else class="card empty-card pa-4">Empty</v-card>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-dialog v-model="dialog" persistent>
          <v-card class="pa-2">
            <v-row>
              <v-col>
                <h3>Background Color Picker</h3>
              </v-col>
              <v-col>
                <h3>Text Editor</h3>
              </v-col>
            </v-row>
            <v-row>
              <v-col class="d-flex justify-center">
                <v-color-picker v-model="editorCardBgColor"></v-color-picker>
              </v-col>
              <v-col>
                <TextEditor :editor-content="editorContent" ref="editor" />
              </v-col>
            </v-row>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialog = false">Close</v-btn>
              <v-btn color="blue darken-1" text @click="saveEditor">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </v-container>
    <v-alert
      max-width="400px"
      transition="slide-x-transition"
      v-model="showAlert"
      type="success"
    >Block component updated</v-alert>
  </v-container>
</template>

<script>
import draggable from "vuedraggable";
import axios from "axios";
import TextEditor from "../components/TextEditor";

export default {
  name: "Box",
  props: ["editable"],
  components: {
    draggable,
    TextEditor
  },
  data() {
    return {
      localBoxContents: [],
      showAlert: false,
      dialog: false,
      editorContent: "",
      editingBlockPosition: 0,
      editorCardBgColor: ""
    };
  },
  async mounted() {
    let response = await axios.get(
      `${process.env.VUE_APP_API_BASE_URL}/blocks`
    );
    console.log(response.data);
    this.localBoxContents = response.data;
    console.log("API BASE URL: " + process.env.VUE_APP_API_BASE_URL);
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
    cancel: async function() {
      let response = await axios.get(
        `${process.env.VUE_APP_API_BASE_URL}/blocks`
      );
      console.log(response.data);
      this.localBoxContents = response.data;
      this.sort();
    },
    sort: function() {
      this.localBoxContents = this.localBoxContents.sort(
        (a, b) => a.position - b.position
      );
    },
    originalOrder: function() {
      this.localBoxContents.splice(0, this.localBoxContents.length);
      this.localBoxContents = [
        { position: 1, text: "", active: false, bg_color: "#ffffff" },
        { position: 2, text: "petotum.com", active: true, bg_color: "#ffffff" },
        { position: 3, text: "", active: false, bg_color: "#ffffff" },
        { position: 4, text: "Build Trust", active: true, bg_color: "#ffffff" },
        { position: 5, text: "", active: false, bg_color: "#ffffff" },
        { position: 6, text: "", active: false, bg_color: "#ffffff" },
        {
          position: 7,
          text: "Pet care dashboard",
          active: true,
          bg_color: "#ffffff"
        },
        { position: 8, text: "", active: false, bg_color: "#ffffff" },
        {
          position: 9,
          text: "Pet Parent Dashboard",
          active: true,
          bg_color: "#ffffff"
        },
        { position: 10, text: "", active: false, bg_color: "#ffffff" },
        { position: 11, text: "", active: false, bg_color: "#ffffff" },
        { position: 12, text: "", active: false, bg_color: "#ffffff" },
        { position: 13, text: "", active: false, bg_color: "#ffffff" },
        { position: 14, text: "", active: false, bg_color: "#ffffff" },
        { position: 15, text: "", active: false, bg_color: "#ffffff" },
        {
          position: 16,
          text: "Provide Transparency",
          active: true,
          bg_color: "#ffffff"
        }
      ];
      this.sort();
    },
    save: async function() {
      for (let i = 0; i < this.localBoxContents.length; i++) {
        this.localBoxContents[i].position = i + 1;
      }
      console.log(JSON.stringify(this.localBoxContents));

      await axios.put(
        `${process.env.VUE_APP_API_BASE_URL}/blocks`,
        this.localBoxContents
      );
      this.showAlert = true;
      console.log("Hide");
      // `event` is the native DOM event
      window.setTimeout(() => {
        this.showAlert = false;
        console.log("hide alert after 3 seconds");
      }, 3000);
    },
    saveEditor() {
      console.log(this.$refs.editor.content);
      console.log(this.editorCardBgColor);

      this.updateBlockContent(
        this.editingBlockPosition,
        this.$refs.editor.content,
        this.editorCardBgColor
      );

      this.dialog = false;
    },
    editBlockText(position, text, bgColor) {
      console.log(`Editing block text on position ${position}, ${text}`);
      this.editorContent = text;
      this.editingBlockPosition = position;
      if (this.$refs.editor) {
        this.$refs.editor.setContentToEdit(text);
      }
      this.editorCardBgColor = bgColor;
      this.dialog = true;
    },
    updateBlockContent(position, text, bgColor) {
      let index = this.localBoxContents.findIndex(
        element => element.position == position
      );
      let blockObj = this.localBoxContents[index];
      blockObj.text = text;
      blockObj.bg_color = bgColor;

      this.localBoxContents.splice(index, 1, blockObj);
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
