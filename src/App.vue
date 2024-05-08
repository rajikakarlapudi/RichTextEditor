<template>
  <div id="app">
    <RichTextEditor />
  </div>
</template>

<script>
import RichTextEditor from './components/RichTextEditor.vue';

export default {
  name: 'App',
  components: {
    RichTextEditor
  }
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  margin: 20px;
}
</style>
