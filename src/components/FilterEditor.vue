<template>
  <div class="container-fluid filter-editor">
    <div class="row">
      <div class="col-md-12">
        
        <p class="filtereditor-style">FilterEditorVUE</p>
        
        <p>FilterEditor for actionType: {{ localFilter.actionType }} Name: {{ localFilter.name }} Code: {{ localFilter.code }}</p>
        <textarea class="form-control" rows="1" v-model="localFilter.name"></textarea>
        <!-- Toggle Disabled -->
        <input type="checkbox" v-model="localFilter.disabled" id="disabled" title="rrr">
        <!--<textarea class="form-control" rows="10" v-model="code"></textarea>-->

        <codemirror
          v-model="localFilter.actionConfig.code"
          :options="cmOption"      />

        <button class="btn btn-danger" @click="deleteFilter">Delete filter</button>
      </div>
    </div>
  </div>

</template>
<script>
  import { Codemirror } from 'vue-codemirror'

  export default {
    name: 'FilterEditor',
    components: {
      Codemirror
    },
    props: {
      filter: Object
    },
    data() {
      return {
        localFilter: JSON.parse(JSON.stringify(this.filter)),
       
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
      }
    },
    methods: {
      deleteFilter() {
        this.$emit('deleteFilter');
      },
      emitChanges() {
        this.$emit('filterChanged',this.localFilter);
      },

      log(eventType, event) {
        console.log(eventType, event);
      }
    },
    watch: {
      name() {
        this.emitChanges();
      },
      code() {
        this.emitChanges();
      },
      disabled() {
        this.emitChanges();
      }
    }
  };
</script>

<style scoped>
.filter-editor{
  background-color: rgb(220, 220, 220);
  text-align: left;

}
.filtereditor-style {
  background-color: rgb(42, 126, 165);
}
</style>