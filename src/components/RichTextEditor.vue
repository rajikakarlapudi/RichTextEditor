<template>
  <div class="app-container">
    <div class="spacer"></div>
    <div class="editor-container">
      <div ref="editor" class="editor" contenteditable="true"></div>
      <div class="toolbar">
        <button @click="bold"><b>B</b></button>
        <button @click="italic"><i>I</i></button>
        <button @click="underline"><u>U</u></button>
        <button @click="strike"><s>S</s></button>
        <button @click="insertBullet">&#8226;</button>
        <button @click="insertNumber">&#35;</button>
        <button @click="insertLink">Link</button>
        <button @click="openCodeEditor">Code</button>
        <input type="color" @change="changeFontColor">
        <select @change="changeFontFamily">
          <option value="">Font Family</option>
          <option value="Arial">Arial</option>
          <option value="serif">serif</option>
          <option value="Verdana">Verdana</option>
          <option value="Times New Roman">Times New Roman</option>
          <option value="Georgia">Georgia</option>
          <option value="Courier New">Courier New</option>
          <option value="Tahoma">Tahoma</option>
        </select>
        <select @change="changeFontSize" v-model="selectedFontSize">
          <option value="">Font Size</option>
          <option value="12pt">12pt</option>
          <option value="14pt">14pt</option>
          <option value="16pt">16pt</option>
          <option value="18pt">18pt</option>
          <option value="20pt">20pt</option>
          <option value="22pt">22pt</option>
          <option value="24pt">24pt</option>
          <option value="26pt">26pt</option>
          <option value="28pt">28pt</option>
          <option value="30pt">30pt</option>
          <option value="32pt">32pt</option>
          <option value="34pt">34pt</option>
          <option value="36pt">36pt</option>
          <option value="38pt">38pt</option>
          <option value="40pt">40pt</option>
        </select>
        <button @click="alignLeft"><i class="fa fa-align-left"></i></button>
        <button @click="alignCenter"><i class="fa fa-align-center"></i></button>
        <button @click="alignRight"><i class="fa fa-align-left"></i></button>
        <label for="image-upload" class="image-upload-label">Upload Image</label>
        <input id="image-upload" type="file" accept="image/*" @change="uploadImage" style="display: none;">
    
        <button @click="saveContent">Save</button>
       
      </div>
    </div>
    <CodeEditorModal @save-code="saveCodeToSavedContent" ref="codeEditorModal" :style="{ fontSize: selectedFontSize, 'important': true }"></CodeEditorModal>

    <div class="saved-content">
      <h2>Saved Content</h2>
      <ul>
        <li v-for="(content, index) in savedContent" :key="index">{{ content }}</li>
      </ul>
    </div>
  </div>
</template>
<script>
import CodeEditorModal from './CodeEditorModal.vue';



export default {
  components: {
    CodeEditorModal
  },
  data() {
    return {
      savedContent: [] ,      // Initialize savedContent array here
      selectedFontSize: ''

    };
  },
 
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
    insertNumber() {
      document.execCommand('insertOrderedList', false, null);
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
    },
    openCodeEditor() {
      this.$refs.codeEditorModal.$refs.textarea.value = ''; // Clear any previous code
      this.$refs.codeEditorModal.$refs.textarea.focus(); // Focus on the textarea
      this.$refs.codeEditorModal.$el.style.display = 'block'; // Show the modal
    },
    saveContent() {
      if (this.$refs.editor.innerHTML.trim() === '') {
        alert('Please enter text before saving.');
        return;
      }
      const editorContent = this.$refs.editor.textContent;
      // Assuming you have a data property called savedContent which is an array
      this.savedContent.push(editorContent);
      this.$refs.editor.textContent="";
    },
    saveCodeToSavedContent(code) {
      if (code.trim() === '') {
        alert('Please enter code before saving.');
        return;
      }
      this.savedContent.push(code);
      this.$refs.codeEditorModal.$refs.textarea.value = ''; // Clear the textarea

      // Add code content to savedContent array
      this.$refs.codeEditorModal.$el.style.display = 'none'; // Hide the modal after saving
    },
    insertCode(code) {
      document.execCommand('insertText', false, code); // Insert code into editor
      this.$refs.codeEditorModal.$el.style.display = 'none'; // Hide the modal after inserting code
    },
    changeFontFamily(event) {
      console.log('Font family changed:', event.target.value);
      const fontFamily = event.target.value;
      document.execCommand('fontName', false, fontFamily);
    },
   
    changeFontSize(event) {
      const fontSize = event.target.value;
  const editor = this.$refs.editor;
  editor.style.fontSize = fontSize; // Set font size directly on the editor
  this.selectedFontSize = fontSize;
    },

    alignLeft() {
      document.execCommand('justifyLeft', false, null);
    },
    alignCenter() {
      document.execCommand('justifyCenter', false, null);
    },
    alignRight() {
      document.execCommand('justifyRight', false, null);
    },
    changeFontColor(event) {
      const fontColor = event.target.value;
      document.execCommand('foreColor', false, fontColor);
    },
    uploadImage(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          const img = document.createElement('img');
          img.src = e.target.result;
          this.$refs.editor.appendChild(img);
        };
        reader.readAsDataURL(file);
      }
    }
   
   


  }
, mounted() {  
    // Hide the code editor modal when the component is mounted
    this.$refs.codeEditorModal.$el.style.display = 'none';
  }
};
</script>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 100%;
  height: 100%;
  padding: 25px;
  background-size: cover;
}
.spacer{
  height: 40px;
}

.editor-container {
  display: flex;
  flex-direction: column;
  width: 75%;
}

.rich-text-editor {
  display: flex;
  flex-direction: column;
  border: 1px solid #000000;
  border-radius: 5px;
  margin: 20px 0; /* Updated margin */
}

.toolbar {
  background-color: #f0f0f0;
  border-top: 1px solid #000000; /* Updated border */
  padding: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 5px;
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
  padding: 20px;
  min-height: 150px;
  border: 2px solid #000000; 
  border-radius: 5px;
}

.editor img,
.editor a {
  max-width: 100%;
  margin-bottom: 10px;
}

.save-button{
  background-color: #000000;
  color: #fff;
}


/* Style for Image Upload Button */
.image-upload-label {
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 8px 16px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.image-upload-label:hover {
  background-color: #0056b3;
}
</style>