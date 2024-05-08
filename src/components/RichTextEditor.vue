<template>
  <div class="rich-text-editor">
    <div class="toolbar">
      <button @click="bold"><b>B</b></button>
      <button @click="italic"><i>I</i></button>
      <button @click="underline"><u>U</u></button>
      <button @click="strike"><s>S</s></button>
      <button @click="insertBullet">&#8226;</button>
      <button @click="insertImage">Image</button>
      <button @click="insertLink">Link</button>
    </div>
    <div ref="editor" class="editor" contenteditable="true"></div>
  </div>
</template>

<script>
export default {
  methods: {
    bold() {
      document.execCommand('bold', false, null);
    },
    italic() {
      document.execCommand('italic', false, null);
    },
    underline() {
      document.execCommand('underline', false, null);
    },
    strike() {
      document.execCommand('strikeThrough', false, null);
    },
    insertBullet() {
      document.execCommand('insertUnorderedList', false, null);
    },
    insertImage() {
      const url = prompt('Enter image URL:');
      if (url) {
        const img = document.createElement('img');
        img.src = url;
        this.$refs.editor.appendChild(img);
      }
    },
    insertLink() {
      const url = prompt('Enter URL:');
      if (url) {
        const text = prompt('Enter link text:');
        if (text) {
          const link = document.createElement('a');
          link.href = url;
          link.textContent = text;
          this.$refs.editor.appendChild(link);
        }
      }
    }
  }
};
</script>

<style scoped>
.rich-text-editor {
  border: 1px solid #f6eded;
  border-radius: 5px;
  margin-bottom: 20px;
}
.toolbar {
  background-color: #f0f0f0;
  border-bottom: 1px solid #ccc;
  padding: 5px;
}
.toolbar button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  padding: 5px 10px;
  margin-right: 5px;
  font-size: 16px;
}
.editor {
  padding: 10px;
  min-height: 150px;
  border-top: 1px solid #ccc;
}
.editor img,
.editor a {
  max-width: 100%;
  margin-bottom: 10px;
}
</style>
