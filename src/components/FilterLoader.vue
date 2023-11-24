<template>
  <div class="row">
    <p class="filterloader-style">LoadConfigFileVUE</p>
    <div class="col-md-6">
      <label for="csvFileInput" class="form-label">Load config file</label>
      <input type="file" class="form-control" id="configInput" @change="loadConfigFile" accept=".yml">
    </div>
    
  </div>
</template>

<script>
import yaml from 'js-yaml';


export default {
  name: 'FilterLoader',
  data() {
    return {
      loadedConfig: Object
    };
  },
  emits: ['loadedConfig'],
  computed: {

  },
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
        // Resetear el valor del input después de la carga
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

