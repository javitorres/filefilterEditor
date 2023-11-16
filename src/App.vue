<template>
  <FileFilterEditor msg="Welcome to FileFilter Editor"/>
</template>

<script>
import FileFilterEditor from './components/FileFilterEditor.vue'
import './index.css'

export default {
  name: 'App',
  components: {
    FileFilterEditor
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
