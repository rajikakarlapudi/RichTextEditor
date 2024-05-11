<template>

<div class="code-editor-modal">
    <div class="modal-content">
        <textarea ref="textarea" placeholder="Enter the code here" v-model="code" rows="10" cols="50"></textarea>
        <button @click="saveCode">Save</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        code: ''
      };
    },
    methods: {
      saveCode() {
        this.$emit('save-code', this.code);
      }
    }
  };
  </script>
  
  <style scoped>
  .code-editor-modal {
  display: none; /* Initially hide the modal */
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent background */
  z-index: 9999;
}

.modal-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 20px;
  border-radius: 5px;
}

textarea {
  width: calc(100% - 20px); /* Subtracting padding */
  max-width: 100%; /* Limiting width to 100% of the container */
  height: 200px; /* Set a fixed height or adjust as needed */
  resize: none; /* Prevent textarea resizing */
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  font-family: 'Courier New', Courier, monospace; /* Example font-family */
}

button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
  </style>
  