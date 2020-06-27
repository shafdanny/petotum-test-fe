<template>
  <div id="app">
    <vue-editor v-model="content" :editorToolbar="customToolbar"></vue-editor>
  </div>
</template>

<script>
import { VueEditor } from "vue2-editor";

export default {
  components: {
    VueEditor
  },
  props: ["editorContent"],
  data() {
    return {
      content: "<p>Some initial content</p>",
      customToolbar: [
        [{ font: [] }],
        [{ header: [false, 1, 2, 3, 4, 5, 6] }],
        ["bold", "italic", "underline", "strike"],
        [
          { align: "" },
          { align: "center" },
          { align: "right" },
          { align: "justify" }
        ],
        [{ header: 1 }, { header: 2 }],
        ["blockquote", "code-block"],
        [{ list: "ordered" }, { list: "bullet" }, { list: "check" }],
        [{ script: "sub" }, { script: "super" }],
        [{ indent: "-1" }, { indent: "+1" }],
        [{ color: [] }, { background: [] }]
      ]
    };
  },
  mounted() {
    this.content = this.editorContent;
  },
  methods: {
    setContentToEdit(content) {
      this.content = content;
    }
  }
};
</script>