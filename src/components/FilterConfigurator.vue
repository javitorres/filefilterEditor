<template>
  <div>
    <div v-for="(filter, index) in filterConfiguration.filters" :key="index" class="mb-3">
    <div class="col-md-6">
      <filter-editor 
        :actionType="filter.actionType"
        @filterChanged="filterChanged(index, $event)"
        @deleteFilter="deleteFilter(index)"
        @testFilter="testFilter(index)">
      </filter-editor><p>NAME: {{ filter.name }} CODE:{{ filter.actionConfig.code }}</p>
    </div>


    <div class="col-md-6">
      <div v-if="csvData.length > 0">
      <h3 class="text-center">Vista Previa del Archivo CSV</h3>
      <table class="table">
        <thead>
          <tr>
            <th v-for="(header, index) in csvHeaders" :key="index">{{ header }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, rowIndex) in displayedCsvData" :key="rowIndex">
            <td v-for="(value, index) in row" :key="index">{{ value }}</td>
          </tr>
        </tbody>
      </table>
    </div>



    </div>


  </div>

    <div class="container text-center" wid>
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

      <!-- show json object -->
      <div class="container text-left">
        
          <div class="col-md-6">
            <pre>{{ JSON.stringify(filterConfiguration, null, 2) }}</pre>
          </div>
        
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
    data() {
      return {
        csvData: [],
        csvHeaders: [],
        filterConfiguration:{
          inDelimiter: ';',
          outDelimiter: ';',
          filterThreads: 1,
          chunkSize: 100000,
          sampleLines: 0,
          filters: []
        }
      };
    },
    methods: {
      addFilter(actionType) {
        this.filterConfiguration.filters.push({ name: "New Filter", actionType: actionType, disabled: false, actionConfig: {} });
        
      },
      updateFilter(index, config) {
        this.filterConfiguration.filters[index].config = config;
      },
      deleteFilter(index) {
        this.filterConfiguration.filters.splice(index, 1);
      },
      testFilter(index) {
        console.log("Testing filter " + index);
      },
      filterChanged(index, filterState) {
        this.filterConfiguration.filters[index].name = filterState.name;
        this.filterConfiguration.filters[index].disabled = filterState.disabled;
        this.filterConfiguration.filters[index].actionConfig.code = filterState.code;
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
