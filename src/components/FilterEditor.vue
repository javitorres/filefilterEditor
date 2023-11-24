<template>
  <div class="container-fluid filter-editor">
    <div class="row">
      <div class="col-md-12">
        
        <p class="filtereditor-style">FilterEditorVUE</p>
        
        <p>FilterEditor for actionType: {{ actionType }}</p>
        <textarea class="form-control" rows="1" v-model="localName"></textarea>
        <!-- Toggle Disabled -->
        <input type="checkbox" v-model="localDisabled" id="disabled" title="rrr">
        <!--<textarea class="form-control" rows="10" v-model="code"></textarea>-->

        <codemirror
          v-model="code"
          :options="cmOption"      />

        <!-- Delete filter button -->
        <button class="btn btn-danger" @click="deleteFilter">Delete filter</button>
        <!-- Test filter -->
        <!--<button class="btn btn-primary" @click="testFilter">Test filter</button>-->
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
      actionType: String,
      name: String,
      disabled: Boolean,
      actionConfig: Object
    },
    data() {
      return {
        localActionType:JSON.parse(JSON.stringify(this.actionType)),
        localName:JSON.parse(JSON.stringify(this.name)),
        localDisabled:JSON.parse(JSON.stringify(this.disabled)),
        localActionConfig:JSON.parse(JSON.stringify(this.actionConfig)),
        code: this.actionConfig.code,
      
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
      testFilter() {
        this.$emit('testFilter');
      },
      emitChanges() {
        this.$emit('filterChanged',{ 
          name: this.name,
          disabled: this.disabled,
          code: this.code,
        }
      );
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