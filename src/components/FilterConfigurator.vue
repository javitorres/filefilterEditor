<template>
  <div>
    <p class="filter-configurator-style">FilterConfiguratorVUE</p>
    <div class="row">
      <div class="col-md-6">
        <div class="mb-3">
          <label for="inDelimiter" class="form-label">In Delimiter</label>
          <input type="text" class="form-control" id="inDelimiter" v-model="localFilterConfiguration.inDelimiter" @change="updateFilterConfiguration">
        </div>
        <div class="mb-3">
          <label for="outDelimiter" class="form-label">Out Delimiter</label>
          <input type="text" class="form-control" id="outDelimiter" v-model="localFilterConfiguration.outDelimiter" @change="updateFilterConfiguration">
        </div>
        <div class="mb-3">
          <label for="filterThreads" class="form-label">Filter Threads</label>
          <input type="number" class="form-control" id="filterThreads" v-model="localFilterConfiguration.filterThreads" @change="updateFilterConfiguration">
        </div>
        <div class="mb-3">
          <label for="chunkSize" class="form-label">Chunk Size</label>
          <input type="number" class="form-control" id="chunkSize" v-model="localFilterConfiguration.chunkSize" @change="updateFilterConfiguration">
        </div>
        <div class="mb-3">
          <label for="sampleLines" class="form-label">Sample Lines</label>
          <input type="number" class="form-control" id="sampleLines" v-model="localFilterConfiguration.sampleLines" @change="updateFilterConfiguration">
        </div>
      </div>
    </div> <!-- end of row -->

    <div v-for="(filter, index) in localFilterConfiguration.filters" :key="index" class="mb-3">
      <div class="col-md-12">
        <filter-editor 
          :actionType="filter.actionType"
          :name="filter.name"
          :disabled="filter.disabled"
          :actionConfig="filter.actionConfig"

          @filterChanged="filterChanged(index, $event)"
          @deleteFilter="deleteFilter(index)"
          @testFilter="testFilter(index)">

        </filter-editor>
      </div>
    </div> <!-- end of v-for -->
    
    <!-- Buttons -->
    <div class="row">
      <div class="col">
        <button class="btn btn-primary" @click="addFilter('python')">Añadir Filtro Python</button>
      </div>
      <div class="col">
        <button class="btn btn-primary" @click="addFilter('pandas')">Añadir Filtro Pandas</button>
      </div>
      <div class="col">
        <button class="btn btn-primary" @click="addFilter('sql')">Añadir Filtro SQL</button>
      </div>
    </div>

  </div>
</template>

<script>
  import FilterEditor from './FilterEditor.vue';

  export default {
    name: 'FilterConfigurator',
    components: {
      FilterEditor
    },
    props:{
      filterConfiguration: {
        inDelimiter: ';',
        outDelimiter: ';',
        filterThreads: 1,
        chunkSize: 100000,
        sampleLines: 0,
        filters: []
      }
    },
    data() {
      return {
        localFilterConfiguration:JSON.parse(JSON.stringify(this.filterConfiguration))
      };
    },
    methods: {
      updateFilterConfiguration() {
        this.$emit('update:filterConfiguration', this.localFilterConfiguration);
      },
      addFilter(actionType) {
        this.localFilterConfiguration.filters.push({ name: "New Filter", actionType: actionType, disabled: false, actionConfig: {} });
        
      },
      updateFilter(index, config) {
        this.localFilterConfiguration.filters[index].config = config;
      },
      deleteFilter(index) {
        this.localFilterConfiguration.filters.splice(index, 1);
      },
      testFilter(index) {
        console.log("Testing filter " + index);
      },
      filterChanged(index, filterState) {
        this.localFilterConfiguration.filters[index].name = filterState.name;
        this.localFilterConfiguration.filters[index].disabled = filterState.disabled;
        this.localFilterConfiguration.filters[index].actionConfig.code = filterState.code;
      }
    },
    watch: {
      filters: {
        handler: function (val) {
          this.$emit('deleteFilter', val);
        },
        deep: true
      }
    }
  };
</script>

<style scoped>
  .filter-configurator-style {
    background-color: rgb(247, 115, 115);
  }
</style>
