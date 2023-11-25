<template>
  <div class="row">
    <p class="filterloader-style">FilterLoaderVUE</p>
    <div class="col-md-6">
      <label for="csvFileInput" class="form-label">Load config file</label>
      <input type="file" class="form-control" id="configInput" @change="loadConfigFile" accept=".yml">
    </div>

    <codemirror
        v-model="loadedConfig"
        :options="cmOption"      />
    
  </div>
</template>

<script>
import yaml from 'js-yaml';
import { Codemirror } from 'vue-codemirror'

export default {
  name: 'FilterLoader',
  components: {
    Codemirror
  },
  data() {
    return {
      loadedConfig: Object,

      cmOption: {
          tabSize: 4,
          styleActiveLine: true,
          lineNumbers: true,
          line: true,
          foldGutter: true,
          styleSelectedText: true,
          mode: 'text/python',
          keyMap: "sublime",
          matchBrackets: true,
          showCursorWhenSelecting: true,
          theme: "monokai",
          extraKeys: { "Ctrl": "autocomplete" },
          hintOptions:{
            completeSingle: false
          }
        }
    };
  },
  emits: ['loadedConfig'],
  methods: {
    loadConfigFile(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();

        reader.onload = (e) => {
          try {
            const loadedConfig = yaml.load(e.target.result);
            console.log("Loaded new config:" + yaml.dump(loadedConfig));
            this.$emit('loadedConfig', loadedConfig);
          } catch (e) {
            console.log(e);
          }
        };

        reader.readAsText(file);
        // Resetear el valor del input después de la carga, de lo contrario si se carga el mismo archivo dos veces, no se disparará el evento
        event.target.value = '';
      }
    }
  }
}
</script>

<style>
.filterloader-style {
  background-color: rgb(42, 126, 165);
}
</style>

