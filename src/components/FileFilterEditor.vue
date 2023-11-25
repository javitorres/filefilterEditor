<template>
  <div class="container-fluid">
    <p class="file-filter-editor-style">FileFilterEditorVUE</p>
    <!--<h1 class="text-center text-muted">Configuration editor</h1>-->

    <div class="row">
      <div class="col-md-6">
        <filter-loader 
        @loadedConfig="loadedConfig => filterConfiguration = loadedConfig"></filter-loader>
      </div>
      
      <div class="col-md-6">
        <csv-input-table></csv-input-table>
      </div>
    </div>

    <div class="row">
      <div class="col-md-12">
        <filter-configurator 
          :filterConfiguration=this.filterConfiguration

          @update="update(newFilterConfiguration)"
          
        > </filter-configurator>
        <!-- QUITO ESTO QUE NO HACE NADA @deleteFilter="newFilters => filterConfiguration.filters = newFilters" -->
      </div>
    </div>
    
    <div class="row">
      <div class="col-md-12">
        <output-section></output-section>
      </div>
    </div>
  </div>
</template>
<script>
import CsvInputTable from './CsvInputTable.vue';
import FilterLoader from './FilterLoader.vue';
import FilterConfigurator from './FilterConfigurator.vue';
import OutputSection from './OutputSection.vue';

export default {
  name: 'FileFilterEditor',
  components: {
    CsvInputTable,
    FilterLoader,
    FilterConfigurator,
    OutputSection
  },
  data() {
    return {
      csvData: [],
      csvHeaders: [],
      displayedCsvData: [],
      filterConfiguration: {
        inDelimiter: ';',
        outDelimiter: ';',
        filterThreads: 1,
        chunkSize: 100000,
        sampleLines: 0,
        filters: []
        
      }
    };
  },
  watch: {
    filterConfiguration() {
      console.log("FilterConfiguration changed: " + JSON.stringify(this.filterConfiguration));
    }
  },
  methods: {
    update(newFilterConfiguration) {
      this.filterConfiguration = newFilterConfiguration;
      console.log("FilterConfiguration changed: " + JSON.stringify(this.filterConfiguration));
      
    }
  }

}
</script>

<style>
.file-filter-editor-style {
  background-color: rgb(42, 165, 79);
}
</style>