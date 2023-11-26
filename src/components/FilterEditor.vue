<template>
  <div class="container-fluid filter-editor">
    <div class="row">
      <div class="col-md-12">
        
        <p class="filtereditor-style">FilterEditorVUE</p>
        
        <p>FilterEditor for actionType: {{ localFilter.actionType }} Name: {{ localFilter.name }} Description: {{ localFilter.description }} Code: {{ localFilter.code }} Disabled: {{ localFilter.disabled }}</p>

        <div class="row">
          <div class="col-md-6">
            <label for="filterName" class="form-label">Filter name</label>
            <textarea class="form-control" rows="1" v-model="localFilter.name"></textarea>
          </div>
          <div class="col-md-6">
            <label for="filterDescription" class="form-label">Filter description</label>
            <textarea class="form-control" rows="1" v-model="localFilter.description"></textarea>
          </div>
        </div>

        <div class="row">
          <div class="col-md-6">
            <!-- Select for action type. Options: python, pandas, sql, rest-->
            <label for="actionType" class="form-label">Action type</label>
            <select class="form-select" v-model="localFilter.actionType">
              <option value="python">Python</option>
              <option value="pandas">Pandas</option>
              <option value="sql">SQL</option>
              <option value="rest">REST</option>
            </select>
          </div>
          <div class="col-md-6">
            <div class="form-check">

              <label class="form-check-label" for="flexCheckDefault">Disabled</label>
              <input class="form-check-input" type="checkbox" value="" id="flexCheckDefault" v-model="localFilter.disabled">
              
            </div>
          </div>
        </div>

        <codemirror
          v-model="localFilter.code"
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
      filterUpdated(filter) {
        this.$emit('filterUpdated', filter);
      },

      log(eventType, event) {
        console.log(eventType, event);
      }
    },
    watch: {
      localFilter() {
        this.filterUpdated(JSON.parse(JSON.stringify(this.localFilter)));
      }
    }
  };
</script>

<style scoped>
  .filter-editor{
    /**background-color: rgb(220, 220, 220);**/
    text-align: left;

  }
  .filtereditor-style {
    background-color: rgb(42, 126, 165);
  }
</style>